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

