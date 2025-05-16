1. Functional & Non‐Functional Requirements

Functional Requirements

    CRUD on Books, Authors, Categories

    Filtering: Books by author, category, publication year

    Pagination & Sorting on collection endpoints

    Hypermedia (HATEOAS) links in responses

    Error reporting with meaningful codes & messages

Non‐Functional Requirements

    JSON over HTTPS

    OAuth2 / JWT Bearer authentication

    Idempotent PUT & DELETE

    ETag-based caching on GETs

    Rate limiting to 100 req/minute

    OpenAPI specification for documentation

2. Domain Entities
| Entity       | Attributes                                                                                               |
| ------------ | -------------------------------------------------------------------------------------------------------- |
| **Book**     | `id: UUID`, `title: string`, `isbn: string`, `publishedDate: date`, `authorId: UUID`, `categoryId: UUID` |
| **Author**   | `id: UUID`, `firstName: string`, `lastName: string`, `bio: string`                                       |
| **Category** | `id: UUID`, `name: string`, `description: string`                                                        |

3. Operations & Endpoints
| Resource       | GET (list)    | GET (item)            | POST          | PUT (item)            | DELETE (item)         |
| -------------- | ------------- | --------------------- | ------------- | --------------------- | --------------------- |
| **Books**      | `/books`      | `/books/{bookId}`     | `/books`      | `/books/{bookId}`     | `/books/{bookId}`     |
| **Authors**    | `/authors`    | `/authors/{authorId}` | `/authors`    | `/authors/{authorId}` | `/authors/{authorId}` |
| **Categories** | `/categories` | `/categories/{catId}` | `/categories` | `/categories/{catId}` | `/categories/{catId}` |

4. Response & Status Codes
| Method                 | Status | Body                                                                          | Caching                               |
| ---------------------- | ------ | ----------------------------------------------------------------------------- | ------------------------------------- |
| **GET /books**         | 200    | `{ content: [Book], page: {number,totalPages,totalElements}, _links: {...} }` | `Cache-Control: max-age=60`           |
| **GET /books/{id}**    | 200    | `{ id,…, _links: { self, author, category, collection } }`                    | `ETag` + `Cache-Control: max-age=120` |
| **POST /books**        | 201    | `{…new book… , _links: { self, collection } }`                                | n/a                                   |
| **PUT /books/{id}**    | 200    | `{…updated book… , _links: { self, collection } }`                            | n/a                                   |
| **DELETE /books/{id}** | 204    | *no body*                                                                     | n/a                                   |

5. Richardson Maturity Model

    Level 0: n/a

    Level 1: distinct URIs for resource types

    Level 2: use of HTTP methods & status codes

    Level 3: HATEOAS—each response includes _links with rel and href, e.g.:

" _links": {
  "self": { "href": "/books/1234" },
  "author": { "href": "/authors/5678" },
  "category": { "href": "/categories/abcd" },
  "collection": { "href": "/books?page=0&size=20" }
}

