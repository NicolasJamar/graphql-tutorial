# Introduction to GraphQL

> NOTE: this tutorial comes from this [Team Treehouse Repo](https://github.com/treehouse-projects/intro-to-graphql)

## Running the Examples

To run the GraphQL examples provided in this repo, start with installing all of the required dependencies by running the following command:

```
npm install
```

To show the Appolo playground, run the following command:


```
node playground.js
```

Then browse to [http://localhost:3000/](http://localhost:3000/), enter your GraphQL query in the left pane, press the "Play" button (in the middle of the screen), and view your results in the right pane.

![Apollo Server](/images/apollo-server.png)

If you click on the green tab that's pinned to the right edge of the screen, you can view the details for the available GraphQL schema.

![Schema](/images/schema-details.png)

For more information about Apollo Server, see the official documentation at [https://www.apollographql.com/docs/apollo-server/](https://www.apollographql.com/docs/apollo-server/).

## GraphQL vs. REST

https://blog.apollographql.com/graphql-vs-rest-5d425123e34b

A RESTful API is one that uses HTTP requests to GET, PUT, POST, and DELETE data. The core idea of REST is the resource. Each resource is identified by a URL. 
The type, or shape, of the resource and the way you fetch that resource are coupled.
The respond could be complex and messy. 
If we want specific data, we have to add an untuitive query string at the end of our request URL OR retreive an object which contains a field which refere to an other object and then send an other request to go deeper in the datas. 

GraphQL has many elements of the REST model built in (like sending an HTTP request) but with GraphQL, you can send queries to get exactly the data you’re looking for in one request. 
In GraphQL, you don’t use URLs to identify what is available in the API. Instead, you use a GraphQL schema.
The type of the resource and the way you fetch that resource are separated.
We could retrieve the same data through many different types of queries, and with different sets of fields.

## Basics terms

* **Fields** - Properties that comprise the shape of a response
* **Type** - A collection of fields that make up a specific queryable object.
* **Scalar field** - A field with a simple data type, such as a String, Int, or Boolean
* **Object field** - A field which returns another object, which can be broken down into further scalar or object type fields
* **Query** - Queries specify which endpoints we want to call, how we want the response to look
* **Mutation** - A special kind of GraphQL query that causes changes to the data available on the backend. It is the equivalent to the POST, PUT, PATCH and DELETE in HTTP/REST speak. 
* **Alias** - A syntax for renaming the result of a field to anything you want
* **Fragment** - Reusable sets of fields that can be included in multiple queries; saves time from needing to manually include the same fields over and over in many places

## basic query structure 

```
query { 
  allMovies 
  {
    id
    title
    tagline
  }
}
```

1. the declaration : query (or mutation)
2. the endpoint : allMovies
3. the fields : data we want to retrieve

## Fragments

```
query ($movieOneId:ID!, $movieTwoId:ID!) {
  movieOne: movieById (movieId: $movieOneId) {
    ...movieDetails
  }
  movieTwo: movieById (movieId: $movieTwoId) {
    ...movieDetails
  }
}

fragment movieDetails on Movie {
  id
  title
  releaseYear
}
```

movieOne and movieTwo are aliases. 
movieDetails is a fragment. 

## Variables in query

Each variable declaration requires two things, a variable name and a variable type. 

A variable is declared with a `$` sign. 

To set a value to a variable, use the *query variable* editor which is written in JSON format:

```
{
	"movieOneId": "movie_0",
  	"movieTwoId": "movie_1"
}
```

## Default variables 

How do you handle queries or mutations where some of your arguments are missing? The answer is default variables. 

```
query ($year:Int = 2000) {
  randomMovieByYear (year: $year) {
    ...movieDetails
  }
}

fragment movieDetails on Movie {
  id
  title
  releaseYear
}
```

Here the default variable is 2000 that we can overwrite if we specify a value in the query variables. 

For example 2001 : 

```
{
  "year": 2001
}
```

## Mutation

Example with create a new movie :

```
mutation {
  createMovie (title: "La cité de la peur", tagline: "ça va couper!" ) {
    id
    title
    tagline
  }
}
```

id, title and tagline are Scalar fields because there are simple fields. 

Example mutation with an objec field :

```
mutation ($directorToAdd: DirectorInput!) {
  addDirectorToMovie (movieId: "movie_4", director:$directorToAdd) {
    title
    tagline
    directors {
      id
      name
    }
  }
}

/** Query variables */
{"directorToAdd": 
  {
  	"name": "Nicolas Jamar"
	}
}
```

directors is an Object field. Objects fields are specifics because they have their own nested properties (fields). 
