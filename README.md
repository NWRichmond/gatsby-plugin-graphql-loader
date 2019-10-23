# gatsby-plugin-graphql-loader

A Gatsby plugin for loading graphql and gql files with graphql-tag/loader.

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
plugins: [
  `gatsby-plugin-graphql-loader`
]
```

Now you can import `.graphql` and `.gql` files directly into your components.

### Example

A working example is show in [this GitHub repository](https://github.com/NWRichmond/gatsby-plugin-graphql-loader-demo).

Suppose you have `src/queries/get-comments-by-first-name.gql` which contains the following query:

```graphql
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
import React from 'react';
import { useQuery } from '@apollo/react-hooks';
import GET_USER_COMMENTS from '../queries/get-comments-by-first-name.gql';

const UserInfo = ({ firstname }) => {
  const { data, loading, error } = useQuery(GET_USER_COMMENTS, {
    variables: { firstname },
  });
  const user = !loading && !error ? data.users[0] : null;
  if (error) {
    return <p>Error Querying Data</p>;
  }
  return (
    error ||
    (!loading && (
      <>
        <h3>First Name</h3>
        <p>{user.firstname}</p>
        <h3>Comments</h3>{' '}
        {user.comments.map(({ text }) => (
          <p key={text.slice(0, 30)}>{text}</p>
        ))}
      </>
    ))
  );
};

export { UserInfo };

```

