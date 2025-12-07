## alx-project-0x14
##  API Overview

The MoviesDatabase API provides structured information about movies, TV series, and episodes. It allows you to search titles, retrieve details, access ratings, browse episodes, fetch actor information, and use helper endpoints such as title types, genres, and curated lists. All responses include a results field, and some endpoints also support pagination.

## Version

v1 — This is the version referenced throughout the MoviesDatabase documentation.

## Available Endpoints
Titles

GET /titles — Returns multiple titles using optional filters and query parameters.

GET /x/titles-by-ids — Returns title details for a list of IMDb IDs supplied as idsList.

GET /titles/{id} — Returns complete information for a single title (movie, series, or episode).

GET /titles/{id}/ratings — Returns rating data, including average score and vote count.

Episodes & Seasons

GET /titles/series/{id} — Returns a list of “light episode” objects for a TV series.

GET /titles/seasons/{id} — Returns the number of seasons for a series.

GET /titles/series/{id}/{season} — Returns all episodes within a specific season.

GET /titles/episode/{id} — Returns full details for a specific episode.

Search

GET /titles/search/keyword/{keyword} — Searches titles by keyword.

GET /titles/search/title/{title} — Searches titles by name, with optional exact matching.

GET /titles/search/akas/{aka} — Searches by alternate titles (exact, case-sensitive).

Upcoming

GET /titles/x/upcoming — Returns a list of upcoming titles.

Actors

GET /actors — Returns a list of actors (supports pagination).

GET /actors/{id} — Returns detailed information about a specific actor.

Utils (Helper Lists)

GET /title/utils/titleType — Returns all valid title types.

GET /title/utils/genres — Returns a list of genres supported by the API.

GET /title/utils/lists — Returns predefined lists such as Top 250 and Most Popular.

## Request and Response Format
General Response Structure
Most endpoints return JSON in this format:
{
  "results": ...,
  "page": 1,     
  "next": 2,      
  "entries": 10   
}

results — main data returned
page, next, entries — included only when pagination applies
Field Selection Using info
The info query parameter controls how much data is returned:
Common values:
mini_info — Basic fields such as id, title, image, and type
base_info — Extended fields (plot, runtime, ratings, etc.)
Additional options include: image, rating, awards, revenue_budget, creators_directors_writers, extendedCast, and more.

Pagination
limit — Items per page (default 10, max 50)
page — Page number (default 1)


## Authentication

The base documentation does not specify a single authentication method, but hosting services (such as RapidAPI) commonly require:

X-RapidAPI-Key: <YOUR_API_KEY>
X-RapidAPI-Host: moviesdatabase.p.rapidapi.com

Some providers may also use:
Authorization: Bearer <API_KEY>

Important:
Always confirm the required authentication method from your API provider dashboard.
Never commit your API key to your repository.
Store keys in environment variables and add .env to your .gitignore.

## Error Handling

Common HTTP responses:

200 OK — Request successful; process results.
400 Bad Request — Invalid query parameter or malformed input.
401 / 403 Unauthorized/Forbidden — Authentication missing or incorrect.
404 Not Found — The requested ID or endpoint does not exist.
429 Too Many Requests — Rate limit reached.
5xx Server Errors — Temporary API issue; retry later.

General handling guidelines:
Validate user input before sending requests.
Show meaningful error messages to the user.
Retry failed requests (especially 429 or 5xx) with a delay.

## Usage Limits and Best Practices

Use limit and page instead of requesting large datasets at once.
Cache data that doesn’t change often (genres, title types, etc.).
Fetch only the fields you need using the info parameter to reduce payload size.
Monitor your request usage to avoid exceeding rate limits.
Handle all HTTP errors gracefully.
Keep your API keys secure and never expose them in client-side code.