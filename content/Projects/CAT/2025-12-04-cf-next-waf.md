source: https://nodeart.app/git/projects/CAT/repos/cat.workers/browse/src/filter-rules-manager/readme.md
# Filter Rules Manager Worker

## Authentication

All endpoints require authentication using a Bearer token in the `Authorization` header:

```
Authorization: Bearer {STRAPI_SITEMAP_CF_KV_SECRET}
```

## Environment Variables

- `STRAPI_SITEMAP_CF_KV_SECRET`: Secret token for API authentication
- `SITEMAP_KV`: KV namespace for storing sitemap urls (motor)
- `MANIFEST_KV`: KV namespace for storing manifest urls (motor)
- `SITEMAP_KV_ATOM`: KV namespace for storing sitemap urls (atom)
- `MANIFEST_KV_ATOM`: KV namespace for storing manifest urls (atom)

## API Endpoints

### GET /urls

Check if a specific URL is allowed in the KV storage.

**Query Parameters:**

- `url` (required): The URL path to check (must start with `/`)
- `product` (required): Product name (`motor` or `atom`)

**Example:**

```bash
GET /urls?url=%2Fsome-page&product=motor
```

**Response:**

```json
{
  "originalUrl": "/some-page",
  "isInKv": true,
  "product": "motor"
}
```

### GET /count

Get the number of keys in a KV namespace.

**Query Parameters:**

- `namespace` (required): The namespace to count keys in (`sitemap` or `manifest`)
- `product` (required): Product name (`motor` or `atom`)

**Example:**

```bash
GET /count?namespace=sitemap&product=motor
```

**Response:**

```json
{
  "namespace": "sitemap",
  "product": "motor",
  "keyCount": 1250
}
```

### POST /urls

Add one or more URLs to the allowed list in KV storage.

**Request Body:**

```json
{
  "urls": ["/page1", "/page2", "/page3"],
  "product": "motor"
}
```

**Parameters:**

- `urls` (required): Array of URL paths to add (each must start with `/`)
- `product` (required): Product name (`motor` or `atom`)

**Response:**

```json
{
  "message": "URL(s) successfully added",
  "originalUrls": ["/page1", "/page2"],
  "processedUrls": [["/page1"], ["/page2"]],
  "product": "motor"
}
```

### DELETE /urls

Remove one or more URLs from the allowed list in KV storage.

**Request Body:**

```json
{
  "urls": ["/page1", "/page2"],
  "product": "motor"
}
```

**Parameters:**

- `urls` (required): Array of URL paths to remove (each must start with `/`)
- `product` (required): Product name (`motor` or `atom`)

**Response:**

```json
{
  "message": "URL(s) successfully removed",
  "originalUrls": ["/page1", "/page2"],
  "processedUrls": [["/page1"], ["/page2"]],
  "product": "motor"
}
```

### POST /manifest

Process a manifest object and populate the MANIFEST_KV namespace with allowed URLs.

**Request Body:**

```json
{
  "manifest": "{\"key1\": \"/page1\", \"key2\": \"/page2\", \"key3\": \"invalid\"}",
  "product": "motor"
}
```

**Parameters:**

- `manifest` (required): JSON.stringify() of an object containing URL paths
- `product` (required): Product name (`motor` or `atom`)

**Note:** The manifest should be a JSON.stringify() of an object containing URL paths.

**Process:**

1. Parses the manifest string as JSON
2. Extracts all string values that start with `/` from the manifest object
3. Adds each URL to the appropriate MANIFEST_KV namespace using `addUrlSimple`

**Note:** In KV we skip already exist links and set new one after each release

**Response:**

```json
{
  "message": "Manifest processed successfully",
  "product": "motor",
  "totalUrls": 2,
  "successfulAdds": 2,
  "failedAdds": 0,
  "results": [
    {
      "url": "/page1",
      "success": true,
      "error": null
    },
    {
      "url": "/page2",
      "success": true,
      "error": null
    }
  ]
}
```

### DELETE /sitemap

Clear all entries from the SITEMAP_KV namespace.

**Query Parameters:**

- `product` (required): Product name (`motor` or `atom`)

**Example:**

```bash
DELETE /sitemap?product=motor
```

**Response:**

```json
{
  "message": "Sitemap keys were deleted successfully",
  "product": "motor",
  "clearedEntries": 1000
}
```

### POST /sitemap-single

Process a single sitemap file with chunking support for large sitemaps.
For now, we do not use index sitemap, because CF has limit with 1000 Keys per request.

**Request Body:**

```json
{
  "url": "https://example.com/sitemap.xml",
  "chunkIndex": 0,
  "maxUrlsPerRequest": 800,
  "product": "motor"
}
```

**Parameters:**

- `url` (required): The sitemap URL to process
- `product` (required): Product name (`motor` or `atom`)
- `chunkIndex` (optional): Starting chunk index (default: 0)
- `maxUrlsPerRequest` (optional): Maximum URLs per chunk (default: 800)

**Response:**

```json
{
  "message": "Sitemap chunk 1 processed successfully",
  "sitemapUrl": "https://example.com/sitemap.xml",
  "product": "motor",
  "totalUrls": 2500,
  "chunkProcessed": 1,
  "hasMoreChunks": true,
  "totalChunks": 4,
  "maxUrlsPerRequest": 800
}
```

**Note:** To parse big sitemap, you can use bash script [process_sitemap.sh](process_sitemap.sh)

## Test Curl Commands

### GET /urls

```bash
# Check if a URL is allowed (motor)
curl -X GET "https://your-worker-domain.com/urls?url=%2Fsome-page&product=motor" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET"

# Check if a URL is allowed (atom)
curl -X GET "https://your-worker-domain.com/urls?url=%2Fsome-page&product=atom" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET"
```

### GET /count

```bash
# Count keys in sitemap namespace (motor)
curl -X GET "https://your-worker-domain.com/count?namespace=sitemap&product=motor" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET"

# Count keys in manifest namespace (motor)
curl -X GET "https://your-worker-domain.com/count?namespace=manifest&product=motor" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET"

# Count keys in sitemap namespace (atom)
curl -X GET "https://your-worker-domain.com/count?namespace=sitemap&product=atom" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET"
```

### POST /urls

```bash
# Add multiple URLs (motor)
curl -X POST "https://your-worker-domain.com/urls" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET" \
  -H "Content-Type: application/json" \
  -d '{
    "urls": ["/page1", "/page2", "/page3"],
    "product": "motor"
  }'

# Add multiple URLs (atom)
curl -X POST "https://your-worker-domain.com/urls" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET" \
  -H "Content-Type: application/json" \
  -d '{
    "urls": ["/page1", "/page2", "/page3"],
    "product": "atom"
  }'
```

### DELETE /urls

```bash
# Remove multiple URLs (motor)
curl -X DELETE "https://your-worker-domain.com/urls" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET" \
  -H "Content-Type: application/json" \
  -d '{
    "urls": ["/page1", "/page2"],
    "product": "motor"
  }'

# Remove multiple URLs (atom)
curl -X DELETE "https://your-worker-domain.com/urls" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET" \
  -H "Content-Type: application/json" \
  -d '{
    "urls": ["/page1", "/page2"],
    "product": "atom"
  }'
```

### POST /manifest

```bash
# Process manifest object (motor)
curl -X POST "https://your-worker-domain.com/manifest" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET" \
  -H "Content-Type: application/json" \
  -d '{
    "manifest": "{\"key1\": \"/page1\", \"key2\": \"/page2\", \"key3\": \"invalid\"}",
    "product": "motor"
  }'

# Process manifest object (atom)
curl -X POST "https://your-worker-domain.com/manifest" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET" \
  -H "Content-Type: application/json" \
  -d '{
    "manifest": "{\"key1\": \"/page1\", \"key2\": \"/page2\", \"key3\": \"invalid\"}",
    "product": "atom"
  }'
```

### DELETE /sitemap

```bash
# Clear all sitemap entries (motor)
curl -X DELETE "https://your-worker-domain.com/sitemap?product=motor" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET"

# Clear all sitemap entries (atom)
curl -X DELETE "https://your-worker-domain.com/sitemap?product=atom" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET"
```

### POST /sitemap-single

```bash
# Process single sitemap file (motor)
curl -X POST "https://your-worker-domain.com/sitemap-single" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com/sitemap.xml",
    "chunkIndex": 0,
    "maxUrlsPerRequest": 800,
    "product": "motor"
  }'

# Process sitemap with custom parameters (motor)
curl -X POST "https://your-worker-domain.com/sitemap-single" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com/sitemap.xml",
    "chunkIndex": 2,
    "maxUrlsPerRequest": 500,
    "product": "motor"
  }'

# Process single sitemap file (atom)
curl -X POST "https://your-worker-domain.com/sitemap-single" \
  -H "Authorization: Bearer YOUR_STRAPI_SITEMAP_CF_KV_SECRET" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com/sitemap.xml",
    "chunkIndex": 0,
    "maxUrlsPerRequest": 800,
    "product": "atom"
  }'
```

## Error Responses

All endpoints return standardized error responses:

```json
{
  "message": "Error description",
  "error": "ERROR_CODE"
}
```

Common error codes:

- `UNAUTHORIZED` (401): Missing or invalid Authorization header
- `FORBIDDEN` (403): Wrong auth secret
- `METHOD_NOT_ALLOWED` (405): Invalid HTTP method
- `BAD_REQUEST` (400): Invalid request parameters or body
- `NOT_FOUND` (404): Endpoint not found
- `INTERNAL_SERVER_ERROR` (500): Server error