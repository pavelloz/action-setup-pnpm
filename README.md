# `wyvox/action-setup-pnpm`

Correctly sets up node, pnpm, and cache for pnpm dependencies so that installation can be as fast as it can be.

## Usage:

```yaml
steps:
  - uses: actions/checkout@v3
  - uses: wyvox/action-setup-pnpm@v2
```

## Support:

- dependency cache
- pnpm only
  - choose `pnpm` version via `package.json#packageManager` or `package.json#volta.pnpm`
  - pass any args

## Options

While this action is meant to reduce boilerplate,
you end up having _one_ extra line than optimal if you need to customize anything,
unless doing inline yaml-object syntax -- example:

```yaml
- uses: wyvox/action-setup-pnpm@v2
  with: { node-version: 18 }
```

### `node-version`

Allows changing the `node-version` passed to `actions/setup-node`.

```yaml
- uses: wyvox/action-setup-pnpm@v2
  with:
    node-version: 18
```

### `node-registry-url`

Allows changing the `registry-url` passed to `actions/setup-node`.

```yaml
- uses: wyvox/action-setup-pnpm@v2
  with:
    node-registry-url: "https://registry.npmjs.org"
```

### `pnpm-version`

Allows changing the `pnpm-version` passed to `pnpm/action-setup`.

```yaml
- uses: wyvox/action-setup-pnpm@v1
  with:
    pnpm-version: 7.29.0
```

### `args`

Passes through any args directly to `pnpm install`.

```yaml
- uses: wyvox/action-setup-pnpm@v2
  with:
    args: '--ignore-scripts --fix-lockfile'
```

## Why?

[`pnpm/action-setup`](https://github.com/pnpm/action-setup/) can install dependencies on its own, but then no cache is used from [`actions/setup-node`](https://github.com/actions/setup-node).

[`actions/setup-node`](https://github.com/actions/setup-node) configures node, (and correctly respects `volta` configurations), cache, etc

To set this up on your own, you would required _three_ manual steps in your workflow config file:

```yaml
steps:
  # ...
  - uses: pnpm/action-setup@v2
    with:
      version: 8
  - uses: actions/setup-node@v3
    with:
      cache: 'pnpm'
  - run: pnpm install
```
