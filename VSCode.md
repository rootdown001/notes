# Extensions

| **Extension**                   | **Purpose**                         | **Keys**                            | **Action**                                |
| ------------------------------- | ----------------------------------- | ----------------------------------- | ----------------------------------------- |
| **Auto Close Tag**              |                                     |                                     | enabled                                   |
| **Auto Rename Tag**             |                                     |                                     | enabled                                   |
| **Bracket colorization toggle** | turn on/off colorization            | CMD+Shift+p                         | toggle "bracket colorization"             |
| **CSS Peak**                    |                                     | Ctrl+Shift+F12                      | Peek                                      |
|                                 |                                     | F12                                 | Go To                                     |
|                                 |                                     | Ctrl+Hover                          | Hover                                     |
| **ES7+ React/Redux**            |                                     |                                     | enabled                                   |
| **HTML Boilerplate**            | insert html boilerplate             | "html......"                        | select                                    |
| **JavaScript Debugger**         |                                     |                                     |                                           |
| **Live Server**                 |                                     | CMD+Shift+p                         | toggle                                    |
| **solidity**                    |                                     |                                     |                                           |
| **ES Lint**                     |                                     | see docs...                         |                                           |
| **Git Graph**                   | view graph of git                   | Git Graph icon in source control    | toggle                                    |
| **TODO Highlight**              | toggle highlight on/off             | CMD+Shift+p                         | toggle "TODO-Highlight: Toggle highlight" |
|                                 | toggle LIST of highlights           | CMD+Shift+p                         | toggle "TODO Highlight: List Annotations" |
|                                 |                                     | TODO: FIXME: keywords               |                                           |
| **Turbo Console Log**           | insert log message in next line     | Hover over Variable + Ctrl-Option-L | insert console.log                        |
|                                 | Comment all console.log from ext    | Options+Shift+c                     | comment all console.log                   |
|                                 | Un-comment all console.log from ext | Options+Shift+u                     | uncomment all console.log                 |
|                                 | Delete all console.log from ext     | Options+Shift+d                     | delete all console.log                    |
| **Prettier**                    | format ALL code with prettier       | CMD+Shift+p                         | toggle "Format Document"                  |
| \*\* Read doc                   | format SELECTED code with prettier  | Highlight code + CMD+Shift+p        | toggle "Format Selection"                 |
| Comments (not an extension)     | comment (multiple) highlighted      | CMD+/                               | comment highlighted code                  |
|                                 | un-comment (multiple) highlighted   | CMD+/                               | un-comment highlighted code               |
|                                 | comment (block) highlighted         | Shift+Option+A                      | block comment highlighted                 |
|                                 | un-comment (block) highlighted      | Shift+Option+A                      | block comment highlighted                 |

# npm

## npm - Vite

| **Command**                                | **Options**     | **Notes**                        |
| ------------------------------------------ | --------------- | -------------------------------- |
| npm create vite@latest                     |                 |                                  |
| [project name:] . OR project name          | npm run dev     | . uses folder you are in         |
| [choose "react"]                           | npm run build   |                                  |
| [choose "javascript" OR "javascript + SWC" | npm run preview | only shows built version of site |
| cd "[project name]"                        |                 |                                  |
| npm i                                      |                 |                                  |
| npm run dev                                |                 |                                  |

## npm - json-server (run FAKE API from json files)

[json-server] (https://www.npmjs.com/package/json-server)

#### Create from scratch...

1.  Install JSON Server

`npm install -g json-server`

2.  Create a db.json file with some data

```json
{
  "posts": [{ "id": 1, "title": "json-server", "author": "typicode" }],
  "comments": [{ "id": 1, "body": "some comment", "postId": 1 }],
  "profile": { "name": "typicode" }
}
```

3.  Start JSON Server

`json-server --watch db.json`

4.  Now if you go to http://localhost:3000/posts/1, you'll get

```json
{ "id": 1, "title": "json-server", "author": "typicode" }
```

#### If already created...

1.  Create API folder with .json files
2.  In API folder, run...
    - npm i
    - npm run dev
3.  Gives a server at http://127.0.0.1:3000 to use to run fake API

## gitignore

Standard gitignore created by vite

```
# Logs
logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*
lerna-debug.log*

node_modules
dist
dist-ssr
*.local

# Editor directories and files
.vscode/*
!.vscode/extensions.json
.idea
.DS_Store
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?

```

## pnpm

installed pnpm to be able to use @t3-oss

did `npm install -g pnpm`

now can use @t3-oss to set @ as a directive

## Tailwind

Use these instructions to set up Tailwind in React project in Vite

[Install Tailwind w/ Vite](https://tailwindcss.com/docs/guides/vite)

- in `main.jsx` do `import "./index.css"`

- creates `tailwind.config.js`

To make @tailwind error in index.css resolve, edit VS Code `settings.json`

- Add

```
{
  "files.associations": {
    "*.css": "tailwindcss"
  }
}
```

## @ directive for paths

first install `path`

`npm install path`

in `vite.config.ts` import `path`, and add alias

```
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react-swc";
import path from "path";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
      "@backend": path.resolve(__dirname, "../api/src"),
    },
  },
});
```

Then in `tsconfig.js`, add following code under // Linting

```
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"],
      "@backend/*": ["../api/src/*"]
    }
```

Now can use @ as directive to "./src"

## create site from GitHub temple

- `gh` was added with `brew install gh`

- In VSCODE terminal, cd to where you want project to be

- In github, choose `templete site`, use as template

- Give it name of what you want project called

- then under `Code`, copy `gh` code from GitHub CLI

- In VSCODE terminal, paste code

- Site is created from template

## update Node & npm

I have `nvm` installed

For latest Node version, run - `nvm install node`

For latest `npm` run `nvm install --latest-npm`
