# HTTP 431 Error

Recently had an issue where a NextJS 13 project was returning HTTP 431 errors for some users. The 431 is a headers too large error. https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/431. The project had a lot of marketing third-party plugins which resulted in a large number of cookies, making the cookie header quite large for some users. Some research led to the discovery of a Node default max-header size of 16 KB. This can be overridden with the start up flag --max-http-header-size https://nodejs.org/api/cli.html#--max-http-header-sizesize. Setting this value higher in the startup command resolved the issue.
#node
#http
#nextjs


