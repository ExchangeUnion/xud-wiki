# Developer Guide

This page is intended to help developers who want to work on `xud`.

## Contribution Guidelines

Be sure to read the [Contribution Guidelines](https://github.com/ExchangeUnion/xud/blob/master/CONTRIBUTING.MD) for the repository before starting work or opening a Pull Request.

## Recommended Development Environments

The following development environments are known to be compatible with `xud` and are recommended for developers that are unsure what tools to use.

### Visual Studio Code

[Visual Studio Code](https://code.visualstudio.com/) is a cross-platform code editor that's compatible with most popular programming languages and extensible via a large collection of plug-ins. Consider using the following plugins for working with `xud`.

- [TSLint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint)
- [Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=coenraads.bracket-pair-colorizer)

## Recommended Reading

- [Official TypeScript Documentation](https://www.typescriptlang.org/docs/home.html)
- [Official Node.js Documentation](https://nodejs.org/en/docs/)
- [LND Developer Guide](https://dev.lightning.community/)
- [Official gRPC Documentation](https://grpc.io/docs/)

## Other Tips & Advice

### Auto-restart `xud` on file change

Auto restart on every file change under `dist` folder with `nodemon`:

```
nodemon --watch dist -e js bin/xud
```

With args:

```
nodemon --watch dist -e js bin/xud --lndbtc.disable=true --lndltc.disable=true
```