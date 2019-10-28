# gatsby-plugin-graphql-loader

[![npm version](https://img.shields.io/npm/v/gatsby-plugin-graphql-loader.svg)](https://www.npmjs.com/package/gatsby-plugin-graphql-loader)
[![GitHub issues](https://img.shields.io/github/issues/NWRichmond/gatsby-plugin-graphql-loader)](https://github.com/NWRichmond/gatsby-plugin-graphql-loader/issues)
[![GitHub license](https://img.shields.io/github/license/NWRichmond/gatsby-plugin-graphql-loader)](https://github.com/NWRichmond/gatsby-plugin-graphql-loader/blob/master/LICENSE)

A Gatsby plugin for loading `.graphql` and `.gql` files with the help of graphql-tag/loader.

#### This is for:

Loading queries for browser runtime GraphQL operations, such as with Apollo Client.

#### This is not for:

Loading queries used at build time.

For more information on the distinction between runtime vs. build time, see the [Gatsby docs](https://www.gatsbyjs.org/docs/overview-of-the-gatsby-build-process/).

## Installing

The [graphql-tag](https://www.npmjs.com/package/graphql-tag) package needs to be installed alongside this plugin. To install both:

### Using Yarn

`yarn add gatsby-plugin-graphql-loader graphql-tag`

### Using NPM

`npm i gatsby-plugin-graphql-loader graphql-tag`

## Usage

Add the plugin to `gatsby-config.js`:

```js
// In your gatsby-config.js
plugins: [`gatsby-plugin-graphql-loader`];
```

Now you can import `.graphql` and `.gql` files directly into your components.

### Example

A working example is show in [this GitHub repository](https://github.com/NWRichmond/gatsby-plugin-graphql-loader-demo).

Suppose you have `src/queries/get-comments-by-first-name.gql` which contains the following query:

```graphql
# get-comments-by-first-name.gql
query getUserComments($firstname: String!) {
  users(where: { firstname_eq: $firstname }) {
    firstname
    comments {
      text
    }
  }
}
```

In a component file (say, `/src/components/user-info.js`), import the query from the `.gql` file, make the query with Apollo Client, and render out the information returned by the query:

```js
// user-info.js
import React from "react";
import { useQuery } from "@apollo/react-hooks";
import GET_USER_COMMENTS from "../queries/get-comments-by-first-name.gql";

const UserInfo = ({ firstname }) => {
  const { data, loading, error } = useQuery(GET_USER_COMMENTS, {
    variables: { firstname }
  });
  const [user] = !loading && !error ? data.users : [null];
  if (error) {
    return <p>Error Querying Data</p>;
  }
  return (
    error ||
    (!loading && (
      <>
        <h3>First Name</h3>
        <p>{user.firstname}</p>
        <h3>Comments</h3>{" "}
        <ul>
          {user.comments.map(({ text }, index) => (
            <li key={`${text.slice(0, 10)}-${index}`}>{text}</li>
          ))}
        </ul>
      </>
    ))
  );
};

export { UserInfo };
```
