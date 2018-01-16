# Definition
GraphQL is a query language created by Facebook which provides a common interface between the client and the server for data fetching and manipulations. 

# Features
1. The response format is described in the query and defined by the client instead of the server: they are called client‐specified queries. 
2. The structure of the data is not hardcoded as in traditional REST APIs.
3. GraphQL is not language specific.
4. Strong-typing - GraphQL introduces application level type system. Its a contract between the client and server. The server may use a different internal type system.
5. Useful when client needs flexible response. To avoid extra queries. To avoid massive data transformation. 
6. Client can change response format without any change in backend.
7. REST APIs are resource based. You address your resources with a unique path. eg. ```GET /users/1/friends/```. Its hard to implement advanced requests like ```GET /users/1/friends/1/dogs/1?include=user.name,dog.age```. GraphQL solves this problem.
8. With GraphQL mutation you can manipulate data. GraphQL uses GET for querying and POST for mutation.
9. Caching in GET queries works the same as with the classic HTTP API.
10. GraphQL can work on HTTP, websockets or even mqtt.
11. [Grafitti](https://www.npmjs.com/package/@risingstack/graffiti) is a middleware using to model your existing models into GraphQL schema.