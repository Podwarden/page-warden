# PageWarden

**A self-hosted web development platform with a built-in MCP server, for people who want to build a web app by talking to Claude Code or ChatGPT.**

[![License: Apache-2.0](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE) [![Changelog 0.1.0](https://img.shields.io/badge/changelog-0.1.0-lightgrey.svg)](CHANGELOG.md) [![Install with PodWarden](https://img.shields.io/badge/install-PodWarden%20catalog-0a7.svg)](https://podwarden.com/catalog/pagewarden)

**[Install PageWarden from the PodWarden catalog](https://podwarden.com/catalog/pagewarden)**, then point Claude Code at your deployment:

```bash
claude mcp add --transport http pagewarden https://<your-domain>/_mcp/message
```

You have a model that writes code and a domain where the app should live. Between them sit the steps you keep doing by hand: copy the files over, run the build, restart the server, bolt on a login page. PageWarden puts all of that behind one MCP endpoint. You deploy one stack, the model gets tools to read and write the site, rebuild it and restart it, and you watch the result at your own domain.

## What you get

- A gateway (Caddy) that serves your app at your domain and routes `/_mcp`, `/_auth`, `/_admin` and `/_files` to the platform.
- An MCP server with file tools (`list_files`, `read_file`, `file_info`, `upload_file`, `create_directory`, `move_file`, `delete_file`) and server tools (`rebuild`, `restart`, `server_status`), behind an embedded OAuth 2.1 authorization server: PKCE, dynamic client registration, discovery at `/.well-known/oauth-authorization-server`.
- A runtime for your code. On the Node stacks it is a Node.js container that runs `npm start`, `server.js` or `index.js` from the site directory; a new deployment starts with a small example site.
- Accounts for the people who use your app: a first-run setup wizard, password sign-in (argon2id), sessions, email magic links once SMTP is configured in the admin pages, an audit log, and `/_auth/me` so your app knows who is signed in.

Five install options: Static, Node + SQLite, Node + Postgres, Node + Postgres + Redis, and a Helm chart for Kubernetes. Your code gets `DATABASE_URL` on the Postgres stacks and `REDIS_URL` on the Redis stack.

## Connecting a client

The MCP endpoint is `https://<your-domain>/_mcp/message`. Your client discovers the OAuth endpoints on its own, registers itself and opens an authorization page in your browser; enter the `SIDECAR_SECRET` you set at install. Access tokens last 24 hours, then the client asks you to authorize again. ChatGPT and other clients take the same URL as a remote MCP server (connector).

A good first message: "Check the server status, look at the files on the site, and tell me what is there."

Lost the one-time setup token printed at first boot? Run `/mcp regenerate-setup-token` inside the MCP server container. It works until the admin account exists; after that, sign in normally.

## About this repository

This repository is a landing page. It holds this README, the [changelog](CHANGELOG.md) and the [license](LICENSE), and nothing else. The PageWarden source code is not published here or anywhere else public; the software ships as container images and is installed through the [PodWarden catalog](https://podwarden.com/catalog/pagewarden), where the install options, resource requirements and About page live.

## License

The contents of this repository are under the [Apache License 2.0](LICENSE). Caddy, Node.js, PostgreSQL, Redis, Claude and ChatGPT are trademarks of their respective owners; PageWarden is not affiliated with or endorsed by them.
