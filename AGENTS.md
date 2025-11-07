# AGENTS Guidelines for This Repository

## Repository Overview

Dify Chat is a monorepo built on top of a pnpm workspace. All sub-packages live under the `packages` directory:

- `api`: Node.js client library for the Dify API
- `components`: React component library for Dify (deprecated — do not import or modify without explicit approval)
- `core`: Core abstractions and shared logic
- `docs`: Documentation site, built with Rspress
- `helpers`: Utility functions
- `platform`: Next.js 15 App Router project that handles CRUD operations for app configuration and proxies Dify API calls
- `react-app`: Front-end web client that end users interact with
- `theme`: Theming components and styles shared across the app

## Dependency Management

When you need to install or update dependencies, remember that the project relies on the pnpm workspace catalog protocol. All dependency versions are declared in the `catalog` section of the root `pnpm-workspace.yaml`. Update the version there, then run `pnpm install` from the repository root after switching to the relevant sub-package.

## Styling

Both primary sub-packages (`react-app` and `platform`) use Tailwind CSS, but they rely on different versions:

- `react-app` uses Tailwind CSS v3, with the exact version defined through the pnpm catalog protocol (see the root `pnpm-workspace.yaml`)
- `platform` uses Tailwind CSS v4, with the version declared directly under `dependencies` in its `package.json`

## Development Workflow

After making code changes, you **do not** need to start the development server to verify them. Every page in this application requires authentication, so I will handle validation once your changes are ready.
