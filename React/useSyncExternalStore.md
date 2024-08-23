# React/useSyncExternalStore

Example of using window.matchMedia with useSyncExternalStore to keep component up to date with browser size.

```
import * as React from "react";
import { phone, desktop } from "./icons";

const query = "only screen and (max-width : 768px)";

const getSnapshot = () => {
  return window.matchMedia(query).matches;
};

  const subscribe = (callback) => {
  const matchMedia = window.matchMedia(query);
  matchMedia.addEventListener("change", callback);

  return () => {
    matchMedia.removeEventListener("change", callback);
  };
};

export default function MatchMedia() {
  const isMobile = React.useSyncExternalStore(subscribe, getSnapshot);



  return (
    <section>
      Resize your browser's window to see changes.
      <article>
        <figure className={isMobile ? "active" : ""}>
          {phone}
          <figcaption>Is mobile: {`${isMobile}`}</figcaption>
        </figure>

        <figure className={!isMobile ? "active" : ""}>
          {desktop}
          <figcaption>Is larger device: {`${!isMobile}`}</figcaption>
        </figure>
      </article>
    </section>
  );
}

```
