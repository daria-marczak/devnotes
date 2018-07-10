# Isomorphic / universal application
An application where the server and the client are written in the same language, and where the server renders the first request and the client renders all subsequent ones.

+ Blanket term for multiple interdependent features
+ Often referred to as "universality"
+ Good implementation results in faster development and rendering of apps

## Isomorphic process
1. User requests web page
1. Server queries database and APIs
1. Server asks app for the HTML it would render given the information it has
1. Server sends the rendered HTML to the client
1. Application renders on client's device (after just one HTTP request)
1. User enjoyes the content
1. Client requests JS from server and boots application
1. Client application recognizes pre-rendered HTML and binds to it
1. Application finishes loading several seconds after user got their initial content

## Functional and technical isomorphism
*Functional* – the server renders the HTML on initial page load
*Technical* – code is shared between the front- and back-end of the application

## Advantages of technical isomorphism
+ Less code to write and maintain
+ Fewer languages to learn
+ Congruent data format (JSON)
+ Library can integrate with self

## Advantages of functional isomorphism
+ Faster load times (especially for mobile)
+ Easier to search enfines to index
+ Automatic support for legacy browsers

## Pitfalls and challenges of isomorphism
+ More complex architecture
+ Challenging to troubleshoot
+ Sensitive data on server prone to exposure
+ More points of possible failure

### How sensitive data is exposed?
1. Developer authors client-facing module
1. A dependency of the module requires a file containing secret data
1. Application is bundled and sent to client, secret data and all
1. Attacker searches bundle and accesses private data

## Isomorphic application priorities

|Development|Production|
|:----------|:---------|
|Application updates quickly after source code is updated|Application is not concerned with the updates to source code (in hot reloading you can)|
|Additional round trips to fetch data are of no consequence|Additional roun trips significantly slow application on mobile and greatly increase the odds of user closing the application before it even loads|
|Code needs to be organized, clear and highly traceable|Code should be obfuscated, compact, and not expose sensitive data|
|Tooling is needed to allow developers to see app's internal state|Tooling is needed to track user's activity and expose bugs|

## Isomorphic React
### Code sharing
+ One of the principal advantages of our architecture choices
+ Uses some of the same code for client as for server
+ Server can load client libraries, making server rendering possible

React renders the views and handles routing with react-router. Express is responsible for fetching data to create the initial state. It uses React to render the app on the server. It also runs Webpack instance in development to serve app bundle. Express loads templates and serves static files. Webpack takes the JS and compiles it into ES5 and serves it. It also injects updated modules directly into client. Compiles modules into a single, static ES5 JS file for production.

### Data injection
1. Data is prepared by server
1. Data is then converted to JSON and injected into index
1. Client application has data immediately

It reslts in a longer initial load.

### Server rendering
1. Data is prepared by server
1. HTML for app is rendered on server by passing to app (this can still take a bit of time, though it's usually a lot fastr than data injection)
1. Client application sees rendered view even though data is not yet fetched

|Data injection|Server rendering|
|:-------------|:---------------|
|Server must be capable of handling data fetching|Server must be capable of rendering hte app and fetching data|
|Can result in fastest load time, if he data is simple and the app is complex|Can result in fastest load time, if the data is complex and the app is simple|
|Doesn't work on devices with no JS|Works great on devices with no JS|
|Complex render can cause performance issues on initial load (especially mobile)|In-client rendering may still be slow but end-user sees finalapp right away, and so perceives it as fast|
|Can skip first AJAX request to fetch data|Still needs to fetch data after initial load|

