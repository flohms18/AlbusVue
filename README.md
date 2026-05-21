# AlbusVue

A Vue 3 + Vite application using Ollama for local AI models.

## Prerequisites

- [Node.js](https://nodejs.org/) v20.19.0 or >= v22.12.0
- [Ollama](https://ollama.com/) installed and running locally

## Setup

Install dependencies:

```bash
npm install
```

## Ollama Commands

| Command | Description |
|---|---|
| `ollama serve` | Start the Ollama server |
| `ollama pull gemma3` | Download the `gemma3` base model |
| `ollama pull albus` | Download the `albus` model |
| `ollama create albus -f Modelfile` | Build the `albus` model from the Modelfile |
| `ollama run albus` | Run the `albus` model interactively |
| `ollama list` | List all downloaded models |

## App Commands

| Command | Description |
|---|---|
| `npm run dev` | Start the development server (hot reload) |
| `npm run build` | Build for production |
| `npm run preview` | Preview the production build locally |

## Troubleshooting

**Ollama is not installed**
Download and install it from [https://ollama.com](https://ollama.com), then restart your terminal.

**Ollama server is not running**
Run `ollama serve` in a separate terminal before starting the app.

**Model `albus` not found**
`albus` is a custom model built on `gemma3`. Pull the base model and create `albus` from the Modelfile:
```bash
ollama pull gemma3
ollama create albus -f Modelfile
```

**Port 5173 already in use**
Another process is using the port. Either stop it or run `npm run dev -- --port 5174` to use a different port.

**Node is not installed**
Download and install it from [https://nodejs.org](https://nodejs.org). Choose the LTS version, then restart your terminal and verify with `node -v`.

**npm is not installed**
npm comes bundled with Node.js. If it's missing, reinstall Node from [https://nodejs.org](https://nodejs.org) and verify with `npm -v`.

**Node version mismatch**
Check your version with `node -v`. You need v20.19.0 or >= v22.12.0. Use [nvm](https://github.com/nvm-sh/nvm) to install and switch versions:
```bash
nvm install 22
nvm use 22
```

**`npm install` fails**
Delete `node_modules` and `package-lock.json`, then run `npm install` again:
```bash
rm -rf node_modules package-lock.json && npm install
```

**`npm run dev` command not found**
Dependencies are not installed. Run `npm install` first.

## Getting Started

1. Run `ollama serve` to start the Ollama server
2. Run `ollama pull gemma3` to download the base model
3. Run `ollama create albus -f Modelfile` to build the `albus` model
4. Run `npm install`
5. Run `npm run dev`
6. Open [http://localhost:5173](http://localhost:5173) in your browser
