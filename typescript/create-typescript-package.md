# Create TypeScript package

**Links:**<br/>
https://medium.com/cameron-nokes/the-30-second-guide-to-publishing-a-typescript-package-to-npm-89d93ff7bccd

1. Run `npm init -y`
2. Run `tsc --init`
3. Add `"declaration": true` to the `tsconfig.json`
    - "This tells TypeScript to emit an `.ts` definitions file along with your compiled JavaScript."
4. Add `"types": "index.d.ts"` to the `package.json`
    - "When other people import your library, this tells the TypeScript compiler where to look for your library’s types."
5. Add `dist` folder to `.gitignore`
6. Run `tsc` to build
7. Done

Example of the final `package.json`:

```json
{
  "name": "my-ts-lib",
  "version": "1.0.0",
  "description": "My npm package written in TS",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "tsc"
  },
  "license": "MIT",
  "devDependencies": {
    "typescript": "^2.7.2"
  }
}
```
