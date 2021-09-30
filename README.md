<h1 align="center"></h1>

<div align="center">
  <a href="http://nestjs.com/" target="_blank">
    <img src="https://nestjs.com/img/logo_text.svg" width="150" alt="Nest Logo" />
  </a>
</div>

<h3 align="center">NestJS CLI Schematics for Dynamic Module Generation</h3>

<div align="center">
  <a href="https://nestjs.com" target="_blank">
    <img src="https://img.shields.io/badge/license-MIT-brightgreen.svg" alt="License" />
    <img src="https://badge.fury.io/js/%40nestjsplus%2Fdyn-schematics.svg" alt="npm version" height="18">
    <img src="https://img.shields.io/badge/built%20with-NestJs-red.svg" alt="Built with NestJS">
  </a>
</div>

### Overview

This package contains a set of [NestJS CLI](https://docs.nestjs.com/cli/overview) schematics for generating modules that can be dynamically configured.  The concepts behind the generated code is covered in detail [in this article](https://dev.to/nestjs/advanced-nestjs-how-to-build-completely-dynamic-nestjs-modules-1370).

See [below](#dynamic-package-generation) for a description of the use cases.

### Installation

Install the package **globally** as shown below.  Due to the implementation of the NestJS CLI, the package **must** be global.

```bash
npm install -g @nestjsplus/dyn-schematics
```

### Dynamic Package Generation

Currently, there are two use cases supported, each with a corresponding schematic:

1. Generating a complete, new standalone NestJS *package* containing a dynamic module. Such a package can easily be installed locally with npm, or packaged and distributed via the [npm registry](https://npmjs.com), or a private registry.  In other words, use this to build a re-usable library.
2. Adding a generated dynamic module *to an existing NestJS project*. In this case, the module is created in its own folder, and is wired in to the existing project using appropriate module metadata, includes, etc.  This schematic works much like Nest's built-in `module` schematic, but creates a fully implemented *dynamic module*.

### Nest CLI

These schematics are built on top of the [Nest CLI](https://docs.nestjs.com/cli/usages) infrastructure, so their usage is exactly as documented on that page.

> Note: since these schematics are built on top of the Nest CLI, all of the optional arguments (such as specifying an optional path in which to generate the code) and options (such as `--dry-run`, `--flat`) are available.  Currently, however, the schematics do not generate spec files.

> Note: I'm working on schematics to add new components to an existing dynamic module, such as additional *options providers*. This should be coming soon.

### Use-case #1: Generating a standalone package

The following step will create a new folder using `<pkg-name>`, which will contain the standalone package files and folders for your new dynamic module package.

#### Use the `dynpkg` schematic

```bash
nest g -c @nestjsplus/dyn-schematics dynpkg <pkg-name>
```

- `dynpkg` is the name of the schematic used to generate a new standalone package containing a dynamic module.
- `<pkg-name>` is the name of the new package you're building.

The schematic will prompt you asking whether to create a *test client*.  If you answer yes, it will add a small module, which you can later easily remove, to test out the newly generated schematic.  I recommend you choose `yes` when first testing out the schematic.

Move to the sub-folder just created:

```bash
cd <pkg-name>
```
- where `pkg-name` is the name you supplied in the original `nest g` command above.

Install the dependencies for the generated package:

```bash
npm install
```

#### Verify generated package

If you answered `yes` to the prompt `Generate a testing client?`, a small testing module was automatically generated called `<pkg-name>ClientModule`.  You can test that the template was properly generated by running:

```bash
npm run start:dev
```

Then browse to [http://localhost:3000](http://localhost:3000).  Your browser should display `Hello from <pkg-name>Module!`.

#### Optionally publish package

The `package.json` and `tsconfig.json` files are generated according to the process described [in this article](https://dev.to/nestjs/publishing-nestjs-packages-with-npm-21fm).  This means that publishing the package to npm is as simple as:
1. updating the `package.json` with your author information, etc.
2. running `npm publish`

See [the npm packaging](https://dev.to/nestjs/publishing-nestjs-packages-with-npm-21fm) article for more information.

### Use-case #2: Adding a dynamic module to an existing project

Make sure you're in the project root folder, just as you would be if running something like `nest g controller myController`.

#### Use the `dnymod` schematic

```bash
nest g -c @nestjsplus/dyn-schematics dynmod <module-name>
```

- `dynmod` is the name of the schematic used to generate a new dynamic module (which will be added to your existing project).
- `<module-name>` is the name of the new module you're building.

The schematic adds the new module in a folder named `<module-name>`.  Just like using the built-in module schematic (e.g., `nest g module myNewModule`), this will add the generated module to the imports list of your root module with the appropriate metadata and includes.  At this point, you can customize the generated module as needed.

### Customizing

The files in the project have comments that should help guide you.

#### More help

You can also refer to these articles:

[How to build completely dynamic NestJS mdoules](https://dev.to/nestjs/advanced-nestjs-how-to-build-completely-dynamic-nestjs-modules-1370) - for details on the concepts behind the *dynamic module pattern*.

[Built a NestJS module for Knex.js in 5 minutes](https://dev.to/nestjs/build-a-nestjs-module-for-knex-js-or-other-resource-based-libraries-in-5-minutes-12an) - an end-to-end tutorial on using these schematics.


### Change Log

See [Changelog](CHANGELOG.md) for more information.

### Contributing

Contributions welcome! See [Contributing](CONTRIBUTING.md).

### Author

**John Biundo (Y Prospect on [Discord](https://discord.gg/G7Qnnhy))**

### License

Licensed under the MIT License - see the [LICENSE](LICENSE) file for details.