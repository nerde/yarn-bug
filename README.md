# Bug: installing dependencies in production is removing a Webpacker dependency

Given the following `package.json`:

```json
{
  "name": "yarn_bug",
  "dependencies": {
    "@rails/webpacker": "^4.0.2"
  },
  "version": "0.1.0",
  "devDependencies": {
    "webpack-dev-server": "^3.3.1"
  }
}
```

When I run `NODE_ENV=production yarn install`, the Webpacker's dependency `file-loader` gets added to `node_modules`,
just as expected.

Now if I add this specific dev dependency: `yarn add --dev @storybook/react`, I get the following `package.json`:


```json
{
  "name": "yarn_bug",
  "dependencies": {
    "@rails/webpacker": "^4.0.2"
  },
  "version": "0.1.0",
  "devDependencies": {
    "@storybook/react": "^5.2.8",
    "webpack-dev-server": "^3.3.1"
  }
}
```

That looks correct. Now if I run `NODE_ENV=production yarn install` again, `file-loader` gets removed from
`node_modules`, this is **not** expected.

Why installing production dependencies after adding that **dev** dependency is getting rid of `file-loader`?

```
➭ node -v
v10.17.0

➭ yarn -v
1.21.0
```

My OS is `macOS Catalina 10.15.1 (19B88)`.

I tested this with `npm` and the issue does **not** occur.
