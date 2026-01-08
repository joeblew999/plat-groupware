# Stalwart Mail Server

Integration with [Stalwart](https://github.com/stalwartlabs/stalwart) - an open-source, secure, and modern all-in-one mail server written in Rust.

## Features

- **SMTP** - Send and receive email (port 2525)
- **IMAP4** - Access mailboxes from any client (port 1143)
- **JMAP** - Modern JSON-based mail protocol
- **CalDAV/CardDAV/WebDAV** - Calendar and contacts sync
- **Built-in spam/phishing filter** with machine learning
- **DKIM, SPF, DMARC, ARC** - Email authentication
- **Web-based admin interface** (port 8085)

## Quick Start

```sh
task stalwart:start              # Build (if needed) and start server
task stalwart:stop               # Stop server
task stalwart:api:health         # Check server health
```

## Tasks

### Server Lifecycle

| Task | Description |
|------|-------------|
| `stalwart:start` | Start mail server |
| `stalwart:stop` | Stop mail server |
| `stalwart:deps:install` | Build from source (requires Rust) |
| `stalwart:deps:clean` | Remove binaries and source |
| `stalwart:config:init` | Generate config from template |

### CLI Commands

Requires `STALWART_ADMIN_PASSWORD` in `.env.local`.

| Task | Description |
|------|-------------|
| `stalwart:cli -- <cmd>` | Run any stalwart-cli command |
| `stalwart:cli:queue` | List message queue |
| `stalwart:cli:report` | List DMARC/TLS reports |

### REST API

| Task | Description |
|------|-------------|
| `stalwart:api:health` | Check server health |
| `stalwart:api:metrics` | Get server metrics |
| `stalwart:api:principals` | List users/domains |
| `stalwart:api:queue:status` | Get queue status |
| `stalwart:api:logs` | Get recent logs |

## REST API Endpoints

Base URL: `http://localhost:8085/api`

### Authentication
- `POST /oauth` - Obtain OAuth token

### Principals (Users/Domains)
- `GET /principal` - List all
- `POST /principal` - Create new
- `GET /principal/{id}` - Get details
- `PATCH /principal/{id}` - Update
- `DELETE /principal/{id}` - Delete

### Queue Management
- `GET /queue/status` - Check queue status
- `GET /queue/messages` - List queued messages
- `PATCH /queue/status/start` - Resume delivery
- `PATCH /queue/status/stop` - Halt processing

### Monitoring
- `GET /telemetry/metrics` - Performance metrics
- `GET /logs` - Query logs

## Configuration

Config template: `config.toml.tmpl`
Generated config: `.data/stalwart/config.toml`

Set in `.env`:
```sh
STALWART_HTTP_PORT=8085
STALWART_SMTP_PORT=2525
STALWART_IMAP_PORT=1143
STALWART_LOG_LEVEL=info
STALWART_VERSION=v0.15.3
```

Set in `.env.local` (secrets):
```sh
STALWART_ADMIN_PASSWORD=your-secure-password
```

## Building from Source

See [Compile Instructions](https://stalw.art/docs/development/compile).

Feature flags available:
- **Data stores**: rocks, sqlite, foundationdb, postgres, mysql
- **Blob storage**: s3, azure
- **Full-text search**: elastic
- **Message queues**: nats, zenoh, kafka

Default build uses RocksDB for all storage.

## Ports

| Port | Protocol |
|------|----------|
| 8085 | HTTP (Admin/JMAP) |
| 2525 | SMTP |
| 1143 | IMAP |

## Links

- [Stalwart GitHub](https://github.com/stalwartlabs/stalwart)
- [Documentation](https://stalw.art/docs/get-started)
- [Management API](https://stalw.art/docs/api/management/endpoints/)
- [CLI Reference](https://stalw.art/docs/management/cli/overview)
- [Compile Instructions](https://stalw.art/docs/development/compile)
