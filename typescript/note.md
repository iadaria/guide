# typescipt in project

### 1
> npm install --save-dev ts-node nodemon
> touch src/index.ts
> touch nodemon.js
`{
  "watch": ["src"],
  "ext": ".ts,.js",
  "ignore": [],
  "exec": "ts-node ./src/index.ts"
}`
- Add to project.json -> "start:dev": "nodemon",
> npm run start:dev

### 2
> npm install typescript --save-dev
> npm install @types/node --save-dev
- create tsconfig.json
> npx tsc --init --rootDir src --outDir build --esModuleInterop --resolveJsonModule --lib es6 --module commonjs --allowJs true --noImplicitAny true
> mkdir src
> touch src/index.ts
> npx tsc