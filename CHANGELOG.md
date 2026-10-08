# Changelog

PageWarden releases are installed from the
[PodWarden catalog](https://podwarden.com/catalog/pagewarden). The source code
is not published in this repository; this file records what each release
changes for people who run PageWarden.

## [0.1.0]

First release tracked in this changelog. PageWarden at this point provides:

- Five install options: Static, Node + SQLite, Node + Postgres,
  Node + Postgres + Redis, and a Helm chart for Kubernetes.
- An MCP server with file tools (`list_files`, `read_file`, `file_info`,
  `upload_file`, `create_directory`, `move_file`, `delete_file`) and server
  tools (`rebuild`, `restart`, `server_status`).
- An embedded OAuth 2.1 authorization server for MCP clients: discovery at
  `/.well-known/oauth-authorization-server`, dynamic client registration and
  PKCE (S256).
- Accounts for your app: first-run setup wizard, password sign-in with
  argon2id, sessions, email magic links (SMTP configured in the admin pages),
  and an audit log.
- `regenerate-setup-token`, a command to mint a new first-run setup token if
  the one printed at first boot was lost.

Fixes included in this release:

- Concurrent sign-ins no longer risk running the MCP server out of memory:
  password hashing is serialized.
- Two simultaneous submissions of the setup form now return a clear refusal
  instead of a server error.
- The Helm chart routes `/_admin`, `/_auth` and the OAuth discovery path
  correctly, keeps the MCP server's data on a persistent volume, and gives the
  MCP server enough memory for password hashing.
