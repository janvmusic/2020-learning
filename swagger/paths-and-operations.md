## Paths and Operations
- In API terms: 
  - **Paths** are endpoints(resources). In example: `/users` or `/reports/summary`
  - **Operations** are HTTP methods. In example: GET, POST, etc.

### Paths
- Paths and operations are defined in `paths` section of the API specification
```yaml
paths:
  /ping:
    ...
  
  /users:
    ...

  /users/{id}
    ...
```
- All paths are relative to API server URL
- **Global servers** can be overridden at `paths` level
- `paths` might have an optional summary and description can be longer

### Path Templating
- To declare variables in the path, you can use **curly braces** `{}`
```yaml
/users/{id}
  ...

/organizations/{orgId}/members/{memberId}
  ...

/report.{format}
  ...
```
- Just as _Rails_ the url needs to be called passing the argument. For example `/users/5` or `/report.xml`

### Operations
- For each `path` you will need to add a `HTTP` operation
- OpenAPI 3.0 supports following operation
  - get
  - post
  - put
  - patch
  - delete
  - head
  - options
  - trace
- One `path` can support multiple operations
- OpenAPI defines that to use multiple operations needs to be a different `key -> value`
```yaml
paths:
  /users/{id}:
    get:
      tags:
        - Users
      summary: Gets a user by ID
      description: Brief description
      operationId: getUserById
      parameters:
        - name: id
          in: path
          description: User id
          required: true
          schema:
            type: integer
            format: int64
      responses:
        '200':
          description: Successful operation
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
      externalDocs:
        description: Learn more
        url: http://example.com
```
- `tags` keyword is used to group operations logically by resources or any other qualifier

### Operation parameters
- **Operations** can receive parameters via `path`, `query string`, `headers` and `cookies`
- They can also be included in `request body` such as POST, PUT, PATCH

### Query String in paths
- Incorrect usage:
```yaml
paths:
  /users?role={role}
```
- Correct usage:
```yaml
paths:
  /users:
    get:
      parameters:
        - in: query
          name: role
          schema:
            type: string
            enum: [user, poweruser, admin]
          required: true
```
- As you can notice, that it's impossible to have multiple paths that differ only in a query string
```
GET /users?firstName=value&lastName=value
GET /users?role=value
```

### operationId
- `operationId` is an optional unique string, used to identify an operation.
- If provided, these IDs must be unique
```yaml
/users:
  get:
    operationId: getUsers
    summary: Gets all users
    ...
  post:
    operationId: addUser
    summary: add a user
    ...

/user/{id}
  get:
    operationId: getUserById
    summary: Gets a user by ID
    ...
```

### Deprecated operations
- You can mark one operation as deprecated
- Use keyword: `deprecated`
```yaml
/pet/findByTags:
  get:
    deprecated: true
```

