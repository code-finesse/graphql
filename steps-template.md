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
  
1. create a mutation to register a user

    ```graphql
    const typeDefs = gql`
        type Query {
            hello: String!
        }

        type Mutation {
            register: User
        }
    `;
    ```

    ---
    
2. create a schema for a user and put in all of the fields the user will have inside typeDefs

    ```graphql
    type User {
            id: ID!
            username: String!
        }
    ```

    - id is used on the frontend with apollo for the cache
    
    ---
    
3. add the mutation to the resolvers

    ```graphql
    Mutation: {
            register: () => ({
                id: 1,
                username: 'bob'
            })
        }
    ```

    - just a dummy example
    
    ---
    
4. check it in the browser

    ```graphql
    # request

    mutation {
      register {
        id
        username
      }
    }

    # response

    {
      "data": {
        "register": {
          "id": "1",
          "username": "bob"
        }
      }
    }
    ```

    ---
    
5. create a schema for register response

    ```graphql
    type Error {
        field: String!
        message: String!
    }

    type RegisterResponse {
    		# brackets around type makes it an array
    		# you can also make all fields non-nullable if you want eg. [Error!]!
        error: [Error]
        # User type is nested
        user: User
    }

    type Mutation {
        register: RegisterResponse!
    }
    ```

    - we also created an Error type in case there are errors and set it to an array in case there were multiple errors
    
    ---
    
6. set up register mutation

    ```graphql
    Mutation: {
            register: () => ({
                errors: [{
                    field: 'username',
                    message: 'bad'
                },
                {
                    field: 'username2',
                    message: 'bad2'
                },
            ],
                user: {
                    id: 1,
                    username: 'bob'
                }
            })
        }
    ```

    - in each object we need to tell it which fields we want
    
    ---
    
7. check it in the browser

    ```graphql
    # request

    mutation {
      register {
        errors {
          field
          message
        }
        user {
          id
          username
        }
      }
    }

    # response

    {
      "data": {
        "register": {
          "errors": [
            {
              "field": "username",
              "message": "bad"
            },
            {
              "field": "username2",
              "message": "bad2"
            }
          ],
          "user": {
            "id": "1",
            "username": "bob"
          }
        }
      }
    }
    ```

    - if you get rid of a field in our request you will not receive any of that type in your response
    
    ---
    
8. add arguments to register

    ```graphql
    type Mutation {
            # give me a specific user
            # the username and passwords are mandatory
            # the age is sometimes required
            register(username: String!, password: String!, age: Int): RegisterResponse!
        }
    ```

    ----
    
9. check it out in your browser

    ```graphql
    # request

    mutation {
       register(username: "spongebob", password: "gary") {
        user{
          id
        }
      }
    }

    # response

    {
      "data": {
        "register": {
          "user": {
            "id": "1"
          }
        }
      }
    }
    ```

    - graphql playground will also tell you exactly what you need to pass in
    
    ---
    
10. create an input (object that takes arguments) to register a user

    ```graphql
    input UserInfo {
            username: String!
            password: String!
            age: Int
        }

    type Mutation {
            # use the input above for argument in register
            register(userInfo: UserInfo!): RegisterResponse!
    				login(userInfo: UserInfo): Boolean!
        }
    ```

    - you can put the mutation fields to put into something you can use multiple times
    
    ---
    
11. call it in the browser

    ```graphql
    # request

    mutation {
       register(userInfo: {username: "spongebob", password: "gary"}) {
        user{
          id
        }
      }
    }

    # response

    {
      "data": {
        "register": {
          "user": {
            "id": "1"
          }
        }
      }
    }
    ```

    ---
    
12. update the query so we can read users

    ```graphql
    # in typeDefs
    type Query {
            hello: String!
            user: User
        }

    # in resolvers
    Query: {
    		hello: () => 'Hello World!',
    		user: () => ({
    			id: 1,
    			username: 'bob',
    		}),
    	},
    ```

    ---
    
13. check again in the browser

    ```graphql
    # request

    query {
      hello
      user {
        id
        username
      }
    }

    # response

    {
      "data": {
        "hello": "Hello World!",
        "user": {
          "id": "1",
          "username": "bob"
        }
      }
    }
    ```


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
