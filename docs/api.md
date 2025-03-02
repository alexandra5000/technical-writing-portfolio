---
layout: custom_default
title: "Movie recommendation API reference"
date: 2025-03-02
tags: [technical writing, api]
permalink: /api/
---

# Movie recommendation API reference

The **Movie recommendation API** provides users with personalized movie suggestions based on genres, ratings, and user preferences. This API can be integrated into applications to offer dynamic movie recommendations.

## Base URL

```
https://api.movierecs.com/v1
```

## Authentication

All requests require an API key. You can obtain one by registering at our [API portal](https://api.movierecs.com/register).

### Authentication example

Include the API key as a query parameter or in the request headers.

**Query parameter authentication**

```http
GET /recommendations?api_key=YOUR_API_KEY
```

**Header authentication**

```http
GET /recommendations
Headers: {
  "Authorization": "Bearer YOUR_API_KEY"
}
```

## Endpoints

Available **Movie recommendation API** endpoints:

* [Get movie recommendations](#get-movie-recommendations)
* [Search for movies](#search-for-movies)
* [Get movie details](#get-movie-details)
* [Add a movie review](#add-a-movie-review)
* [Delete a review](#delete-a-review)
* [Update user preferences](#update-user-preferences)

### Get movie recommendations

#### Request

```http
GET /recommendations?user_id={user_id}&genre={genre}&limit={limit}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `user_id` | String | Yes | The unique identifier of the user. |
| `genre` | String | No | Genre filter (e.g., `action`, `comedy`). Defaults to all genres. |
| `limit` | Int | No | Number of recommendations to return (default: 5). |

#### Response

```json
{
  "user_id": "12345",
  "recommendations": [
    {
      "title": "Inception",
      "genre": "Sci-Fi",
      "rating": 8.8,
      "release_year": 2010
    },
    {
      "title": "The Dark Knight",
      "genre": "Action",
      "rating": 9.0,
      "release_year": 2008
    }
  ]
}
```

### Search for movies

#### Request

```http
GET /search?query={query}&year={year}&limit={limit}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `query` | String | Yes | Search term for movie title. |
| `year` | Int | No | Filter results by release year. |
| `limit` | Int | No | Number of results to return (default: 10). |

#### Response

```json
{
  "results": [
    {
      "title": "Interstellar",
      "genre": "Sci-Fi",
      "rating": 8.6,
      "release_year": 2014
    },
    {
      "title": "Dune",
      "genre": "Sci-Fi",
      "rating": 8.1,
      "release_year": 2021
    }
  ]
}
```

### Get movie details

#### Request

```http
GET /movie/{movie_id}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `movie_id` | String | Yes | Unique identifier of the movie. |

#### Response

```json
{
  "movie_id": "tt1375666",
  "title": "Inception",
  "genre": "Sci-Fi",
  "rating": 8.8,
  "release_year": 2010,
  "director": "Christopher Nolan",
  "cast": ["Leonardo DiCaprio", "Joseph Gordon-Levitt", "Elliot Page"]
}
```

### Add a movie review

#### Request

```http
POST /reviews
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `user_id` | String | Yes | Unique identifier of the user submitting the review. |
| `movie_id` | String | Yes | Unique identifier of the movie. |
| `rating` | Float | Yes | User's rating (0.0 - 10.0). |
| `comment` | String | No | Optional text review. |

#### Request example

```json
{
  "user_id": "12345",
  "movie_id": "tt1375666",
  "rating": 9.0,
  "comment": "Amazing movie!"
}
```

#### Response example

```json
{
  "message": "Review submitted successfully",
  "review_id": "r67890"
}
```

### Delete a review

#### Request

```http
DELETE /reviews/{review_id}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `review_id` | String | Yes | Unique identifier of the review to delete. |

#### Response example

```json
{
  "message": "Review deleted successfully"
}
```

### Update user preferences

#### Request

```http
PUT /users/{user_id}/preferences
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `user_id` | String | Yes | Unique identifier of the user. |
| `genres` | Array | No | List of preferred genres. |
| `min_rating` | Float | No | Minimum rating for recommended movies. |

#### Request example

```json
{
  "genres": ["Sci-Fi", "Thriller"],
  "min_rating": 7.5
}
```

#### Response example

```json
{
  "message": "Preferences updated successfully"
}
```

## Error codes

| Status Code | Meaning | Description |
| --- | --- | --- |
| 400 | Bad Request | Invalid parameters provided. |
| 401 | Unauthorized | Missing or invalid API key. |
| 404 | Not Found | Movie, review, or user not found. |
| 500 | Internal Server Error | An unexpected server issue occurred. |