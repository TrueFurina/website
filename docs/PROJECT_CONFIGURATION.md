# ⚙️ Project Configuration

The application has been bootstrapped using [create-awesome-python-app](https://pypi.org/project/create-awesome-python-app/) for simplicity reasons. It allows us to create applications quickly without dealing with a complex tooling setup such as bundling, transpiling etc.

You should always configure and use the following tools:

## ESLint

ESLint is a linting tool for JavaScript. By providing specific configuration defined in the`eslint.config.mjs` file it prevents developers from making silly mistakes in their code and enforces consistency in the codebase.

[ESLint Configuration](../eslint.config.mjs)

## Prettier

This is a great tool for formatting code. It enforces a consistent code style across your entire codebase. By utilizing the "format on save" feature in your IDE you can automatically format the code based on the configuration provided in the `.prettierrc.js` file. It will also give you good feedback when something is wrong with the code. If the auto-format doesn't work, something is wrong with the code.

[Prettier Configuration](../.prettierrc.js)

## TypeScript

ESLint is great for catching some of the bugs related to the language, but since JavaScript is a dynamic language ESLint cannot check data that run through the applications, which can lead to bugs, especially on larger projects. That is why TypeScript should be used. It is very useful during large refactors because it reports any issues you might miss otherwise. When refactoring, change the type declaration first, then fix all the TypeScript errors throughout the project and you are done. One thing you should keep in mind is that TypeScript does not protect your application from failing during runtime, it only does type checking during build time, but it increases development confidence drastically anyways. Here is a [great resource on using TypeScript with React](https://react-typescript-cheatsheet.netlify.app/).

## Husky

Husky is a tool for executing git hooks. Use Husky to run your code validations before every commit, thus making sure the code is in the best shape possible at any point of time and no faulty commits get into the repo. It can run linting, code formatting and type checking, etc. before it allows pushing the code. You can check how to configure it [Husky documentation](https://typicode.github.io/husky/#/?id=usage).

## Absolute imports

Absolute imports should always be configured and used because it makes it easier to move files around and avoid messy import paths such as `../../../Component`. Wherever you move the file, all the imports will remain intact. Here is how to configure it:

For JavaScript (`jsconfig.json`) projects:

```json
"compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
```

For TypeScript (`tsconfig.json`) projects:

```json
"compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
```

[Paths Configuration](../tsconfig.json)

It is also possible to define multiple paths for various folders(such as `@/components`, `@/hooks`, etc.), but using `@/*` works very well because it is short enough so there is no need to configure multiple paths and it differs from other dependency modules so there is no confusion in what comes from `node_modules` and what is our source folder. That means that anything in the `src` folder can be accessed via `@`, e.g some file that lives in `src/components/MyComponent` can be accessed using `@/components/MyComponents`.

## uv

[uv](https://docs.astral.sh/uv/) is the Python package/project manager used by this
repository (and by `create-awesome-python-app` scaffolds). It manages the virtual
environment, dependencies, and tool versions:

```bash
uv sync          # create the venv and install all dependencies
uv add <pkg>     # add a runtime dependency
uv add --dev <pkg>  # add a development dependency
uv run <cmd>     # run a command inside the project venv
```

Never use `pip install` directly — the lockfile (`uv.lock`) is the source of
truth for the environment.

## pyproject.toml

`pyproject.toml` is the single configuration file for the Python side of the
project. It declares:

- **Project metadata** — name, version, description, authors
- **Dependencies** — the runtime dependency list (managed by `uv add`)
- **Tool configuration** — settings for linters/formatters/tests (e.g. `ruff`,
  `pytest`) live under their own `[tool.*]` tables

When adding a new dependency or a new tool, edit `pyproject.toml` (prefer
`uv add` for dependencies so the lockfile stays in sync) rather than
scattering config across ad-hoc files.

## Environment variables (`.env`)

Runtime configuration lives in environment variables, with a committed
`.env.example` as the template:

```bash
cp .env.example .env   # create your local copy (`.env` is gitignored)
```

Never commit real secrets into `.env` — only the example file is tracked. To
add a new variable: add it to `.env.example` with a one-line comment, read it
in code via `os.environ.get("NAME")` (or your config loader), and document it
in the docs.
