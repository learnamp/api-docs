# Paginated Responses

All "View All" type responses will be paginated.

## Pagination data in Reponse

Pagination data will be available in the following response headers:

Response Header | Example value | Description
--------- | ------- | -----------
Total | 1032 | Total number of matching results
Per-Page | 10 | Number of results per page
Total-Pages | 103 | Total number of pages

## Apply pagination in Request

Pagination data can be set, by adding the following URL params in the request:

Request URL param | Example value | Description
--------- | ------- | -----------
page | 2 | Return the second page of results
perPage | 20 | Return 20 results per page


**Example:**

`GET https://api.learnamp.com/tasks?page=3&perPage=30`
</aside>