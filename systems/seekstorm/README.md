# SeekStorm Search Engine

Integration with [SeekStorm](https://github.com/SeekStorm/SeekStorm) - an open-source, sub-millisecond full-text search library & multi-tenancy server written in Rust.

## Features

- **Sub-millisecond search** - Optimized for speed with SIMD acceleration
- **Full-text lexical search** - Boolean queries (AND, OR, PHRASE, NOT)
- **BM25F ranking** - With proximity-aware scoring
- **Faceted search** - String and numeric range aggregations
- **Geo-proximity** - Location-based search, filtering, and sorting
- **Fuzzy search** - Typo tolerance and spelling correction
- **Multi-tenancy** - API-key management for SaaS deployments
- **Real-time indexing** - True incremental updates
- **Multi-format ingestion** - CSV, JSON, NDJSON, PDF

## Quick Start

```sh
task seekstorm:start              # Build (if needed) and start server
task seekstorm:stop               # Stop server
task seekstorm:api:health         # Check server health
```

## Tasks

### Server Lifecycle

| Task | Description |
|------|-------------|
| `seekstorm:start` | Start search server on port 8086 |
| `seekstorm:stop` | Stop search server |
| `seekstorm:deps:install` | Build from source (requires Rust) |
| `seekstorm:deps:clean` | Remove binary and source |

### REST API

| Task | Description |
|------|-------------|
| `seekstorm:api:health` | Check server health |
| `seekstorm:api:indices` | List all indices |
| `seekstorm:api:create-index -- <name>` | Create new index |
| `seekstorm:api:search INDEX=x QUERY=y` | Search an index |

### Test Data

| Task | Description |
|------|-------------|
| `seekstorm:test:setup` | Create index and load test data |
| `seekstorm:test:create-index` | Create documents index |
| `seekstorm:test:load-data` | Load sample documents |
| `seekstorm:test:search QUERY=x` | Search test data |

## REST API Endpoints

Base URL: `http://localhost:8086/api/v1`

All endpoints require `apikey` header.

### Index Management
- `GET /index` - List all indices
- `POST /index` - Create new index
- `DELETE /index/{name}` - Delete index

### Documents
- `POST /index/{name}/doc` - Index document(s)
- `GET /index/{name}/doc/{id}` - Get document
- `DELETE /index/{name}/doc/{id}` - Delete document

### Search
- `GET /index/{name}/query?query=...` - Search index
- `POST /index/{name}/query` - Search with JSON body

## Configuration

Set in `.env`:
```sh
SEEKSTORM_PORT=8086
SEEKSTORM_VERSION=v0.12.19
```

Set in `.env.local` (secrets):
```sh
MASTER_KEY_SECRET=your-secret-key    # Protects API key generation
SEEKSTORM_API_KEY=your-api-key       # For API access
```

## Test Data

Sample documents are in `testdata/documents.json`:
- 10 sample documents about search, groupware, and documentation
- Fields: id, title, body, category, tags

Load with: `task seekstorm:test:setup`

## Performance

- Sharded search: 7x faster query latency
- Sharded indexing: 35,000 docs/second
- Supports billion-scale indices on single server

## Links

- [SeekStorm GitHub](https://github.com/SeekStorm/SeekStorm)
- [API Documentation](https://seekstorm.apidocumentation.com/reference)
- [Library Docs](https://docs.rs/seekstorm)
