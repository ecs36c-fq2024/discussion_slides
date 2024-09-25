# ECS 36C Fall 2024 Discussions

## Setup

### Prerequisite

- Node.js 18 (LTS)
- pnpm v8+
- VS Code with Marp Extension (recommended)

### Install Project Dependencies

```sh
pnpm i
```

## Run

### Build All

```sh
pnpm build
pnpm build-pptx
pnpm build-pdf
```

### Build One

```sh
pnpm marp discussion.md
pnpm marp discussion.md --pdf
pnpm marp discussion.md --pptx
```

### Start a local server

```sh
pnpm marp --server .
```
