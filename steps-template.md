# Steps for Creating Basic GraphQL Backend

<details>
  <summary>Terms and Explanations</summary>
  
- typeDefs
- resolvers
- type query
    - things that will be read
    - queries run in parallel
- type mutation
    - things that will be created, updated, or deleted
    - mutations run in order
- Resolver
    - a collection of functions that generate response for a GraphQL query. It acts as a GraphQL query handler. Every resolver function in a GraphQL schema accepts four positional arguments as given below − fieldName:(root, args, context, info) => { result }
</details>
<br/>

<details>
  <summary>Set Up</summary>
  
  ## Subheading
1. Initialize and set defaults to yes

    ```bash

    npm init -y

    ```
---

2. Install dependencies

    ```bash

    npm i apollo-server graphql nodemon

    ```
---


3. Create a "src" folder with index.js inside
---

4. Pull in requirements to index.js file

    ```javascript

    const {ApolloServer, gql} = require('apollo-server')

    ```
---

5. Create a schema

    ```graphql

    const typeDefs = gql`
    type Query {
				# (!) exclamation point in type makes it a non-nullable field
        hello: String!
    }
    `;

    ```
    * known as typeDefs
    * parses the string
---


6. Create resolvers

    ```qraphql

    const resolvers = {
    Query: {
        # what we return when hello query is called
        hello: () => 'Hello World!'
      }
    };

    ```
---


7. Create instance of apollo server and set it up

    ```javascript

    const server = new ApolloServer({ typeDefs, resolvers });

    server.listen().then(({url}) => console.log(`server started at ${url}`))
    ```
    * we can make the server run on a specific port by putting an argument in the listen method
---


8. Start server

    ```bash

    # WITHOUT NODEMON
    node src/index.js

    # WITH NODEMON
    nodemon run dev

    ```
    
    ```javascript

    // package.json script with nodemon

    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "dev": "nodemon ./src/index.js"
      },
    ```
---


9. Go to http://localhost:4000/ (or whichecer one you specified)

---


10. EXPLAIN GRAPHQL PLAYGROUND

    - like Postman
    - comes bundled with apollo server
    - allows you to easily access and explore the data
    - schema tab is most important/used
    - you can create new tabs
    - default settings will mostly be enough
    - you can also see a history of commands you've run before
    - it has autocomplete and user friendly error messages
    
---

4. Test your first query

    ```graphql
    # request

    query {
      hello
    }

    # response

    {
      "data": {
        "hello": "Hello World!"
      }
    }
    ```
    - hello responded and we got hello world back from it
    - after each edit you have to restart your sever (unless you're using nodemon)
    - after each new type the browser may need to be refreshed


</details>


<details>
  <summary>Build Out</summary>
  
  ## Subheading
1. Step one for Bash

    ```bash

    $mkdir project

    ```
    * things to note
---

2. Step two for Javascript

    ```javascript

    const str = 'The quick brown fox jumps over the lazy dog.';

    ```
    * things to note
---

3. Step three for Python

    ```python

    str = 'The quick brown fox jumps over the lazy dog.'

    ```
    * things to note
---

4. Step four for Java

    ```java

    String str = 'The quick brown fox jumps over the lazy dog.';

    ```
    * things to note
---

</details>


<details>
  <summary>Extra Functionality</summary>
  
  ## Subheading
1. Step one for Bash

    ```bash

    $mkdir project

    ```
    * things to note
---

2. Step two for Javascript

    ```javascript

    const str = 'The quick brown fox jumps over the lazy dog.';

    ```
    * things to note
---

3. Step three for Python

    ```python

    str = 'The quick brown fox jumps over the lazy dog.'

    ```
    * things to note
---

4. Step four for Java

    ```java

    String str = 'The quick brown fox jumps over the lazy dog.';

    ```
    * things to note
---

</details>
<br/>


<details>
  <summary>Extra Section</summary>
  
  ## If Necessary
  1. A numbered
  2. list
     * With some
     * Sub bullets
</details>
