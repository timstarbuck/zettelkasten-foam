# TanStack Query

#### Example of Query Factory

```JavaScript
import { useQueryClient, useQuery, QueryClient } from "@tanstack/react-query";

import {
  getMyBooks,
  getBook,
  getReviewsForBook,
  getFeaturedBooks,
  getLatestReview,
  getBookSearchResult
} from "./utils";

export const client = new QueryClient();

// This factory combines the Query Key and Function
export const bookQueries = {
  all: () => ({
    queryKey: ["books"]
  }),
  detail: (bookId) => ({
    queryKey: ["books", bookId],
    queryFn: () => getBook(bookId),
    staleTime: Infinity
  }),
  featured: () => ({
    queryKey: ["books", "featured"],
    queryFn: getFeaturedBooks,
    staleTime: Infinity
  }),
  myBooks: () => ({
    queryKey: ["books", "my-books"],
    queryFn: getMyBooks
  }),
  search: (query, page) => ({
    queryKey: ["books", "search", query, page],
    queryFn: () => getBookSearchResult(query, page),
    enabled: Boolean(query),
    placeholderData: (previousData) => previousData
  })
};

export const reviewQueries = {
  all: () => ({
    queryKey: ["reviews"]
  }),
  detail: (bookId) => ({
    queryKey: ["reviews", bookId],
    queryFn: () => getReviewsForBook(bookId)
  }),
  latest: () => ({
    queryKey: ["reviews", "latest"],
    queryFn: getLatestReview
  })
};

export function useBookReviews(bookId) {
  return useQuery(reviewQueries.detail(bookId));
}

export function useBookQuery(bookId) {
  const queryClient = useQueryClient();
  return useQuery({
    ...bookQueries.detail(bookId),
    initialData: () => {
      return queryClient
        .getQueryData(bookQueries.featured().queryKey)
        ?.find((book) => book.id === bookId);
    }
  });
}

export function usePrefetchBookById(bookId) {
  const queryClient = useQueryClient();
  const prefetch = () => {
    queryClient.prefetchQuery(bookQueries.detail(bookId));
  };

  return { prefetch };
}

export function useFeaturedBooks() {
  return useQuery(bookQueries.featured());
}

export function useLatestReview() {
  return useQuery(reviewQueries.latest());
}

export function useMyBooks() {
  return useQuery(bookQueries.myBooks());
}

export function useSearchQuery(query, page) {
  return useQuery(bookQueries.search(query, page));
}
```

#### Mutation using optimistic Fetching

```JavaScript
export function useCheckoutBook(book) {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: () => checkoutBook(book.id),
    onMutate: async () => {
      // Cancel any queries in flight
      await queryClient.cancelQueries(bookQueries.all());

      // get a snapshot to return, fallback on failure
      const snapshot = {
        myBooks: queryClient.getQueryData(bookQueries.myBooks().queryKey),
        bookDetail: queryClient.getQueryData(
          bookQueries.detail(book.id).queryKey
        )
      };

      // optimistically update caches
      queryClient.setQueryData(
        bookQueries.myBooks().queryKey,
        (previousBooks) =>
          previousBooks ? [...previousBooks, book] : undefined
      );

      // optimistically update caches
      queryClient.setQueryData(
        bookQueries.detail(book.id).queryKey,
        (previousBook) =>
          previousBook
            ? {
                ...previousBook,
                isCheckedOutByUser: true,
                availableCopies: previousBook.availableCopies - 1
              }
            : undefined
      );

      return () => {
        queryClient.setQueryData(
          bookQueries.myBooks().queryKey,
          snapshot.myBooks
        );
        queryClient.setQueryData(
          bookQueries.detail(book.id).queryKey,
          snapshot.bookDetail
        );
      };
    },
    onError: (err, _variables, rollback) => {
      console.log(err);
      rollback?.();
    },
    // onSettled invalidate queries
    onSettled: () => {
      return queryClient.invalidateQueries({ queryKey: bookQueries.all() });
    }
  });
}
```

Observers 
: are the glue between the Query Cache and any React component, and they live outside the React component tree.

#### Performance Notes

Can use select option in useQuery to only return data needed. That way if there is data that changes on every call, but isn't used in the component, it won't trigger a re-render.

```JavaScript
const { data, refetch } = useQuery({
  queryKey: ['user'],
  queryFn: () => {
    console.log('queryFn runs')
    return Promise.resolve({
      name: 'Dominik',
      updatedAt: Date.now() // <-- Changes each time
    })
  },
  select: (data) => ({ name: data.name }) // <-- Only the data the component uses is returned
})
```

Don't use the rest operator with useQuery. This is because useQuery uses *tracked properties* to determine if the Observer should re-render the component. i.e. if fetchStatus is referenced, it will re-render when it's value changes. If fetchStatus is not referenced, re-render won't be trigger by it's value changes.

```JavaScript
// DON'T DO THIS !!!!
const { data, ...rest } = useQuery({ queryKey, queryFn })
```

TanStack Query creates an Abort Controller when invoking a queryFn. It then passes the signal as part of the QueryFunctionContext. This can be passed to the fetch request so it will abort when needed. This is useful in situations where many queries are invoked in rapid succession, for example when a user is typing in a search field. This ensures only the last fetch in-flight will be put into the cache.

```JavaScript
function useIssues(search) {
  return useQuery({
    queryKey: ['issues', search],
    queryFn: ({ signal }) => {  //<-- destructure the signal from QueryFunctionContext
      const searchParams = new URLSearchParams()
      searchParams.append('q', `${search} is:issue repo:TanStack/query`)

      const url = `https://api.github.com/search/issues?${searchParams}`

      const response = await fetch(url, { signal }) //<-- pass the signal to fetch

      if (!response.ok) {
        throw new Error('fetch failed')
      }

      return response.json()
    }
  })
}
```
#### Error Handling

Display a message if query is in a retry state.
```JavaScript
 const { status, data, failureCount } = useTodos()

  if (status === 'pending') {
    return (
      <div>
        ...{failureCount > 1  ? <span>(This is taking longer than expected. Hang tight.)</span> : null}

        <i>retry attempts: {failureCount}</i>
      </div>
    )
  }
```

Use thrownOnError option to cause errors to reach ```<ErrorBoundary>``` components.
```JavaScript
function useTodos() {
  return useQuery({
    queryKey: ['todos', 'list'],
    queryFn: fetchTodos,
    retryDelay: 1000,
    throwOnError: true, <---
  })
}
```

Wrap with QueryErrorResetBoundary and pass reset to ErrorBoundary.onReset to return query when resetErrorBoundary is called.

```JavaScript
import { ErrorBoundary } from 'react-error-boundary'
import { QueryErrorResetBoundary } from '@tanstack/react-query'

function Fallback({ error, resetErrorBoundary }) {
  return (
    <>
      <p>Error: { error.message }</p>
      <button onClick={resetErrorBoundary}>
        Try again
      </button>
    </>
  )
}

export default function App() {
  return (
    <QueryErrorResetBoundary>
      {({ reset }) => (
        <ErrorBoundary
          onReset={reset}
          FallbackComponent={Fallback}
        >
          <TodoList />
        </ErrorBoundary>
      )}
    </QueryErrorResetBoundary>
  )
}
```

Default, opinionated, error handling configuration used by TanStack team.

> With this setup, if there's data in the cache and the query goes into an error state, since the user is most likely already seeing the existing data, we show a toast notification. Otherwise, we throw the error to an ErrorBoundary.


```JavaScript
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      throwOnError: (error, query) => {
        return typeof query.state.data === 'undefined'
      }
    }
  },
  queryCache: new QueryCache({
    onError: (error, query) => {
      if (typeof query.state.data !== 'undefined') {
        toast.error(error.message)
      }
    }
  })
})
```

#### Validation

Using Zod to parse/verify responses not only ensures the correct shape, but also strips any properties not included in the schema, making the Query Cache more efficient.
```JavaScript
const bookSchema = z.object({
  id: z.string(),
  title: z.string(),
  thumbnail: z.string(),
  description: z.string(),
  averageRating: z.number().optional(),
  authors: z.array(z.string()).optional()
});

function useBook(bookId) {
  return useQuery({
    queryKey: ["books", { bookId }],
    queryFn: async () => {
      const data = await getBook(bookId);

      return bookSchema.parse(data);
    },
    retry: (failureCount, error) => {    // <---- function to disable retry when there is a validation error
      if (error instanceof z.ZodError) {
        return false;
      }
      return failureCount < 3;
    }
  });
}
```

#### Offline support

The status gives us information about the data in the cache at the queryKey, and the fetchStatus gives us information about the queryFn.
```json
{
  "status": "pending",
  "data": undefined,
  "fetchStatus": "paused"
}
```

networkMode settings

online
: default - if device is offline, will paulse the query and not attempt to execute the queryFn.

always 
: tell React Query to always execute the queryFn, regardless of the network status.

offlineFirst
: if a request has been made and stored in the browser's cache before the device goes offline, React Query will still invoke the queryFn, which will call fetch, getting data from the browser's cache and returning it to React Query. If there's no data in the browser's cache, React Query will pause the query and wait until the device regains connectivity to try again.

##### Persisters
Persisters are an optional plugin that will take whatever is in the query cache and persist it to a more permanent location of your choosing (think localStorage or IndexedDB). Once persisted, as soon as the app loads, the persisted data will be restored to the cache before React Query does anything else.

Plugins
- @tanstack/query-sync-storage-persister plugin (localStorage)
- @tanstack/query-async-storage-persister plugin (IndexedDB)

@tanstack/react-query-persist-client adapter - React specific 

As a rule of thumb, it's a good idea to define the gcTime as the same value or higher than maxAge to avoid your queries being garbage collected and removed from the storage too early:
```JavaScript
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      gcTime: 1000 * 60 * 60 * 12, // 12 hours
    },
  },
})
...
<PersistQueryClientProvider
  client={queryClient}
  persistOptions={{
    persister,
    maxAge: 1000 * 60 * 60 * 12, // 12 hours
  }}
>
```
There is a predefined retry strategy to remove the oldest query if the localStorage fails. Otherwise you can create your own function to only save the most recent, or whatever business requires.

```JavaScript
const persister = createSyncStoragePersister({
  storage: localStorage,
  retry: removeOldestQuery
})
```
There is an experimental createPersister API to declare a persister on a per-query basis rather than the whole client.
```JavaScript
import { useQuery } from '@tanstack/react-query'
import { experimental_createPersister } from '@tanstack/react-query-persist-client'

function usePostList() {
  return useQuery({
    queryKey: ['posts'],
    queryFn: fetchPosts,
    staleTime: 5 * 1000,
    persister: experimental_createPersister({
      storage: localStorage,
    }),
  })
}
```
Query client should have a default mutation function. With the PersistQueryClientProvider you can trigger the resumption in onSuccess.
```JavaScript
queryClient.setMutationDefaults(['posts'], {
  mutationFn: addPost
})

<PersistQueryClientProvider
  client={queryClient}
  persistOptions={{ persister }}
  onSuccess={() => {
    return queryClient.resumePausedMutations()
  }}
>
```

#### Testing

Common function to render a component, wrapped in a QueryClientProvider
```Javascript
function renderWithClient(ui) {
  const testQueryClient = new QueryClient({
    defaultOptions: {
      queries: {
        retry: false
      }
    }
  });

  return render(
    <QueryClientProvider client={testQueryClient}>
      {ui}
    </QueryClientProvider>
  );
}
```

TanStack recommends using [Mock Service Workers](https://mswjs.io/) for mocking API requests.


#### Suspense
With Suspense boundaries, you no longer need to check for isLoading or status. You also can count on the data returned to be present. 
Just use ```useSuspenseQuery``` instead of ```useQuery```.

```Javascript
function useRepoData(name) {
  return useSuspenseQuery({
    queryKey: ['repoData', name],
    queryFn: () => fetchRepoData(name),
  })
}

function Repo({ name }) {
  const { data } = useRepoData(name)

  return (
    <>
      <h1>{data.name}</h1>
      <p>{data.description}</p>
      <strong>👀 {data.subscribers_count}</strong>{" "}
      <strong>✨ {data.stargazers_count}</strong>{" "}
      <strong>🍴 {data.forks_count}</strong>
    </>
  )
}

export default function App() {
  return (
    <AppErrorBoundary>
      <React.Suspense fallback={<p>...</p>}>
        <Repo name="tanstack/query" />
        <Repo name="tanstack/table" />
        <Repo name="tanstack/router" />
      </React.Suspense>
    </AppErrorBoundary>
  )
}
```

==Don't go chasing waterfalls==
> One thing to keep in mind is that Suspense depends on component composition – a component "suspends" as a whole as soon as one async resource (in our case, a Query) is requested.

> If you have a single component that wants to fire off multiple Queries by calling useSuspenseQuery multiple times, those will not run in parallel. Instead, the component will suspend until the first fetch has finished, then it will continue, just to suspend again until the second Query is completed.

> The best way to avoid these waterfalls scenarios with suspense is to stick to one Query per component, or to use another provided hook, useSuspenseQueries, which can fire off multiple suspense Queries in parallel.

You can use React's useTransition hook instead of placeholderData when using useSuspenseQuery to show previous data while the new query is running.

```Javascript
function RepoList({ sort, page, setPage }) {
  const { data } = useRepos(sort, page)
  const [isPreviousData, startTransition] = React.useTransition()

  return (
    <div>
      <ul style={{ opacity: isPreviousData ? 0.5 : 1 }}>
        {data.map((repo) => 
          <li key={repo.id}>{repo.full_name}</li>
        )}
      </ul>
      <div>
        <button
          onClick={() => {
            startTransition(() => {
              setPage((p) => p - 1)
            })
          }}
          disabled={isPreviousData || page === 1}
        >
          Previous
        </button>
        <span>Page {page}</span>
        <button
          disabled={isPreviousData || data?.length < PAGE_SIZE}
          onClick={() => {
            startTransition(() => {
              setPage((p) => p + 1)
            })
          }}
        >
          Next
        </button>
      </div>
    </div>
  )
}
```

#### Server Rendering / Server Components

Serialize the cache and send it to the client where it can be hydrated. To serialize the cache, we'll use React Query's dehydrate function and to hydrate the cache on the client, we'll use React Query's HydrationBoundary component.

```JavaScript
import { QueryClient, dehydrate, HydrationBoundary } from '@tanstack/react-query'
import { fetchRepoData } from './api'
import Repo from './Repo'

export default async function Home() {
  const queryClient = new QueryClient()

  await queryClient.prefetchQuery({
    queryKey: ["repoData"],
    queryFn: fetchRepoData,
    staleTime: 10 * 1000,
  })

  return (
    <main>
      <HydrationBoundary state={dehydrate(queryClient)}>
        <Repo />
      </HydrationBoundary>
    </main>
  );
}
```
