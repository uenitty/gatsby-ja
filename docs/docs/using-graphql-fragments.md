---
title: GraphQL Fragmentsの利用
---

Fragments を使用すると、GraphQL クエリの一部を再利用できます。また、複雑なクエリをより小さく、理解しやすい要素に分割することもできます。

## fragment の構成要素

fragment の例を次に示します：

```graphql
fragment FragmentName on TypeName {
  field1
  field2
}
```

fragment は、3 つの要素で構成されます：

1. `FragmentName`: 後で参照される fragment の名前。
1. `TypeName`: the [GraphQL type](https://graphql.org/graphql-js/object-types/) of the object the fragment will be used on. This is important because you can only query for fields that actually exist on a given object.
1. `TypeName`: fragment が使用されるオブジェクトの [GraphQL type](https://graphql.org/graphql-js/object-types/)。特定のオブジェクトに実際に存在するフィールドのみを照会できるため、これは重要です。
1. クエリの本文。ここでは、GraphQL クエリの他の場所と同じように、任意のレベルのネストを持つ任意のフィールドを定義できます。

## fragment の作成と利用

fragment はどの GraphQL クエリ内にも作成できますが、おすすめは個別にクエリを作成することです。構成に関するその他のアドバイスは[Conceptual Guide](/docs/graphql-concepts/#fragments)をご覧ください。

```jsx:title=src/components/IndexPost.jsx
import React from "react"
import { graphql } from "gatsby"

export default ( props ) => {
  return (...)
}

export const query = graphql`
  fragment SiteInformation on Site {
    siteMetadata {
      title
      siteDescription
    }
  }
`
```

This defines a fragment named `SiteInformation`. Now it can be used from within the page's GraphQL query:
これは `SiteInformation` という名前の fragment を定義します。これで、ページの GraphQL クエリ内から使用できるようになりました。

```jsx:title=src/pages/main.jsx
import React from "react"
import { graphql } from "gatsby"
import IndexPost from "../components/IndexPost"

export default ({ data }) => {
  return (
    <div>
      <h1>{data.site.siteMetadata.title}</h1>
      <p>{data.site.siteMetadata.siteDescription}</p>

      {/*
        Or you can pass all the data from the fragment
        back to the component that defined it
      */}
      <IndexPost siteInformation={data.site.siteMetadata} />
    </div>
  )
}

export const query = graphql`
  query {
    site {
      ...SiteInformation
    }
  }
`
```

When compiling your site, Gatsby preprocesses all GraphQL queries it finds. Therefore, any file that gets included in your project can define a snippet. However, only Pages can define GraphQL queries that actually return data. This is why you can define the fragment in the component file - it doesn't actually return any data directly.

## Further reading

- [Querying Data with GraphQL - Fragments](/docs/graphql-concepts/#fragments)
- [GraphQL Docs - Fragments](https://graphql.org/learn/queries/#fragments)
