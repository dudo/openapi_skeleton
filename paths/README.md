# Paths

The paths section defines individual endpoints (paths) in your API, and the HTTP methods (operations) supported by these endpoints. For example, GET /users can be described as:

```yml
paths:
  /users:
    get:
      summary: Returns a list of users.
      description: Optional extended description in CommonMark or HTML
      responses:
        '200':
          description: A JSON array of user names
          content:
            application/json:
              schema:
                type: array
                items:
                  type: string
```

An operation definition includes parameters, request body (if any), possible response status codes (such as 200 OK or 404 Not Found) and response contents. For more information, see <https://swagger.io/docs/specification/paths-and-operations/>

## Parameters

Operations can have parameters passed via URL path (/users/{userId}), query string (/users?role=admin), headers (X-CustomHeader: Value) or cookies (Cookie: debug=0). You can define the parameter data types, format, whether they are required or optional, and other details:

```yml
paths:
  /user/{userId}:
    get:
      summary: Returns a user by ID.
      parameters:
        - name: userId
          in: path
          required: true
          description: Parameter description in CommonMark or HTML.
          schema:
            type : integer
            format: int64
            minimum: 1
      responses:
        '200':
          description: OK
```

For more information, see <https://swagger.io/docs/specification/describing-parameters/>

## Request Body

If an operation sends a request body, use the requestBody keyword to describe the body content and media type.

```yml
paths:
  /users:
    post:
      summary: Creates a user.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                username:
                  type: string
      responses:
        '201':
          description: Created
```

For more information, see <https://swagger.io/docs/specification/describing-request-body/>

## Responses

For each operation, you can define possible status codes, such as 200 OK or 404 Not Found, and the response body schema. Schemas can be defined inline or referenced via [$ref](https://swagger.io/docs/specification/using-ref/). You can also provide example responses for different content types:

```yml
paths:
  /user/{userId}:
    get:
      summary: Returns a user by ID.
      parameters:
        - name: userId
          in: path
          required: true
          description: The ID of the user to return.
          schema:
            type: integer
            format: int64
            minimum: 1
      responses:
        '200':
          description: A user object.
          content:
            application/json:
              schema:
                type: object
                properties:
                  id:
                    type: integer
                    format: int64
                    example: 4
                  name:
                    type: string
                    example: Jessica Smith
        '400':
          description: The specified user ID is invalid (not a number).
        '404':
          description: A user with the specified ID was not found.
        default:
          description: Unexpected error
```

Note that the response HTTP status codes must be enclosed in quotes: "200" (OpenAPI 2.0 did not require this). For more information, see <https://swagger.io/docs/specification/describing-responses/>
