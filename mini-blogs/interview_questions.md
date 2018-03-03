# Interview QnAs

1. How to avoid callback hells?
a. Modularization - Break callbacks as independent functions
b. Use control library, like async
c. Use generators with Promises
d. Use async/await

2. What are promises?
Promises are concurrency primitive. Can help you better handle async operations. Promises are chainable.

3. Tools for consistent coding styles?
When working with team, consistent style is important, so that team members can modify more project easily.

4. What's a test pyramid?
A test pyramid describes the ratio of how many unit tests, integration tests and end-to-end test you should write.
a. lots of low-level unit tests for models (dependencies are stubbed)
b. fewer integration tests, where you check how your models interact with each other (dependencies are not stubbed)
c. less end-to-end tests, where you call your actual endpoints (dependencies are not stubbed).

5. How can you make sure your dependencies are safe?
```npm outdated```
[nodesecurity](https://nodesecurity.io/)

6. What is the timing attack ?
A timing attack is a side channel attack in which the attacker attempts to compromise a cryptosystem by analyzing the time taken to execute cryptographic algorithms.
```
function checkApiKey (apiKeyFromDb, apiKeyReceived) {
  if (apiKeyFromDb === apiKeyReceived) {
    return true
  }
  return false
}
```
V8, the JavaScript engine used by Node.js, tries to optimize the code you run from a performance point of view. It starts comparing the strings character by character, and once a mismatch is found, it stops the comparison operation. So the longer the attacker has right from the password, the more time it takes.
To solve this issue, use the npm module cryptiles
```
function checkApiKey (apiKeyFromDb, apiKeyReceived) {
  return cryptiles.fixedTimeComparison(apiKeyFromDb, apiKeyReceived)
}
```