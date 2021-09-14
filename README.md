# example-aws-project

## Project setup
```
npm install
amplify init
amplify add auth
amplify add storage
amplify add API
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

### Note!
I cannot put whole project. There might be needs to run the project but I put everything I guess.

### GraphQL Schema

type Post
  @model
  @auth(rules: [{ allow: owner }])
{
  id: ID!
  owner: ID!
  ownerId: String!
  name: String!
  createdAt: String
  updatedAt: String
  postContents: [PostContent] @connection(name: "PostPostContents", sortField: "createdAt")
}

type PostContent
  @model
  @key(name: "postContentByDate", fields: [ "contentType", "createdAt"], queryField: "postContentsByDate")
  @auth(
    rules: [
      { allow: owner },
      { allow: private, provider: iam, operations: [ read, update, delete ] }
    ]
  )
{
  id: ID!
  createdAt: String
  updatedAt: String
  post: Post @connection(name: "PostPostContents", sortField: "createdAt")
  fullsize: S3Object
  contentType: String!
  text: String!
  size: Int
}

type ProfilePhoto
@model
@key(name: "profilePhotosByDate", fields: [ "owner", "createdAt"], queryField: "profilePhotosByDate")
@auth(
  rules: [
    { allow: owner },
    { allow: private, provider: iam, operations: [ read, update, delete ] }
  ]
)
{
  id: ID!
  createdAt: String
  updatedAt: String
  size: Int
  fullsize: S3Object!
  owner: String!
  ownerId: String!
  contentType: String
  height: Int
  width: Int
}

type S3Object @aws_iam @aws_cognito_user_pools {
  region: String!
  bucket: String!
  key: String!
}


