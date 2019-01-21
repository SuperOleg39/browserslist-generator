<img alt="Logo for @wessberg/polyfiller" src="https://raw.githubusercontent.com/wessberg/browserslist-generator/master/documentation/asset/logo.png" height="200"></img><br>
<a href="https://npmcharts.com/compare/@wessberg/browserslist-generator?minimal=true"><img alt="Downloads per month" src="https://img.shields.io/npm/dm/%40wessberg%2Fbrowserslist-generator.svg" height="20"></img></a>
<a href="https://david-dm.org/wessberg/browserslist-generator"><img alt="Dependencies" src="https://img.shields.io/david/wessberg/browserslist-generator.svg" height="20"></img></a>
<a href="https://www.npmjs.com/package/@wessberg/browserslist-generator"><img alt="NPM Version" src="https://badge.fury.io/js/%40wessberg%2Fbrowserslist-generator.svg" height="20"></img></a>
<a href="https://github.com/wessberg/browserslist-generator/graphs/contributors"><img alt="Contributors" src="https://img.shields.io/github/contributors/wessberg%2Fbrowserslist-generator.svg" height="20"></img></a>
<a href="https://opensource.org/licenses/MIT"><img alt="MIT License" src="https://img.shields.io/badge/License-MIT-yellow.svg" height="20"></img></a>
<a href="https://www.patreon.com/bePatron?u=11315442"><img alt="Support on Patreon" src="https://c5.patreon.com/external/logo/become_a_patron_button@2x.png" height="20"></img></a>

# `@wessberg/browserslist-generator`

> A library that makes generating and validating Browserslists a breeze!

## Description

This is a library that makes it easier to work with [browserslists](https://github.com/browserslist/browserslist).
It can do things like generating a Browserslist that targets only browsers that support - _or don't support_ - specific required features, or even generate a Browserslist from a User Agent string!
It can also do the same in reverse - match a Browserslist on a user agent.
A _Feature_ is anything that can be found on [caniuse](https://caniuse.com/) or [MDN](https://github.com/mdn/browser-compat-data).

## Install

### NPM

```
$ npm install @wessberg/browserslist-generator
```

### Yarn

```
$ yarn add @wessberg/browserslist-generator
```

## Usage

### Generating a Browserslist based on features

When deciding which Browsers and environments to support, it is quite common to make
the decision based on feature support. With this library, you no longer have to neither look up
Browser support and manually write a Browserslist, nor make sure to keep it up-to-date.
Instead, simply declare the features that should be available:

```typescript
import {browsersWithSupportForFeatures} from "@wessberg/browserslist-generator";
// Generate a browserslist for browsers that support all of the given features
const browserslist = browsersWithSupportForFeatures("es6-module", "shadowdomv1", "custom-elementsv1");
```

### Checking if a User Agent supports a specific feature

This library offers simple ways that you can check if a given User Agent supports any amount of features.
This could be useful, among other things, for conditional bundle serving:

```typescript
import {userAgentSupportsFeatures} from "@wessberg/browserslist-generator";
if (userAgentSupportsFeatures(userAgentString, "javascript.builtins.Promise.finally")) {
	doA();
} else {
	doB();
}
```

### Checking if a Browserslist supports a specific feature

Given an existing Browserslist, this library can check if it supports one or more features.
This could be useful, among other things, for conditional bundle serving:

```typescript
import {browserslistSupportsFeatures} from "@wessberg/browserslist-generator";
if (browserslistSupportsFeatures(browserslist, "es6-module")) {
	useModernBundle();
} else {
	useLegacyBundle();
}
```

### Generating a Browserslist based on a ECMAScript version

When deciding which Browsers and environments to support, it is quite common to make
the decision based on a specific version of ECMAScript to target. For example, with the Typescript Compiler,
the `target` option takes an ECMAScript version and the Typescript Compiler then knows which transformations to apply accordingly.

```typescript
import {browsersWithSupportForEcmaVersion} from "@wessberg/browserslist-generator";
// Generate a browserslist for browsers that support the given version of ECMAScript
const browserslist = browsersWithSupportForEcmaVersion("es2015");
```

#### Checking if a Browserslist supports a specific ECMAScript version

Given an existing Browserslist, this library can also check if it supports a specific version of ECMAScript.
This could be useful, among other things, for conditional bundle serving:

```typescript
import {browserslistSupportsEcmaVersion} from "@wessberg/browserslist-generator";
if (browserslistSupportsEcmaVersion(browserslist, "es2015")) {
	useModernBundle();
} else {
	useLegacyBundle();
}
```

#### Getting the most appropriate ECMAScript version for a Browserslist

Given an existing Browserslist, this library can detect the most appropriate ECMAScript version to target.
This could be useful, for example, when using the Typescript compiler based on a Browserslist.

```typescript
import {getAppropriateEcmaVersionForBrowserslist} from "@wessberg/browserslist-generator";

const typescriptOptions = {
	// ...
	target: getAppropriateEcmaVersionForBrowserslist(browserslist)
};
```

#### Possible ECMAScript versions

All of the possible ECMAScript versions are:

- `es3`
- `es5`
- `es2015`
- `es2016`
- `es2017`
- `es2018`

## Contributing

Do you want to contribute? Awesome! Please follow [these recommendations](./CONTRIBUTING.md).

## Maintainers

- <a href="https://github.com/wessberg"><img alt="Frederik Wessberg" src="https://avatars2.githubusercontent.com/u/20454213?s=460&v=4" height="11"></img></a> [Frederik Wessberg](https://github.com/wessberg): _Maintainer_

## FAQ

### What is some cool example of a use case for this library?

Well, here's one I think is pretty neat:
You're building an app, and you care about serving the smallest amount of code to your users.
You've decided to build two bundles: One for browsers _with_, and one for browsers _without_ ES-module support.
You can now generate two Browserslists via `@wessberg/browserslist-generator`:

```typescript
browsersWithSupportForFeatures("es6-module");
browsersWithoutSupportForFeatures("es6-module");
```

Now, you can then pass each one into tools like `@babel/preset-env` and `postcss`.
On the server, you can use the function `userAgentSupportsFeatures` to check if the same features are supported and respond with resources that points to the right bundle.

## Backers 🏅

[Become a backer](https://www.patreon.com/bePatron?u=11315442) and get your name, logo, and link to your site listed here.

## License 📄

MIT © [Frederik Wessberg](https://github.com/wessberg)

## API Reference

##### `browsersWithSupportForFeatures (...features: string[]): string[]`

Takes any amount of [caniuse](https://caniuse.com/) or [MDN](https://github.com/mdn/browser-compat-data) features and generates a [browserslist](https://github.com/browserslist/browserslist) that targets all browsers that support these features

##### `browsersWithoutSupportForFeatures (...features: string[]): string[]`

Takes any amount of [caniuse](https://caniuse.com/) or [MDN](https://github.com/mdn/browser-compat-data) features and generates a [browserslist](https://github.com/browserslist/browserslist) that targets all browsers that _doesn't_ support these features

##### `browsersWithSupportForEcmaVersion (version: EcmaVersion): string[]`

Generates a [browserslist](https://github.com/browserslist/browserslist) that targets all the browsers that support the given ECMAScript version.
See [this section](#possible-ecmascript-versions) for an overview of all supported ECMAScript versions.

##### `browserslistSupportsFeatures (browserslist: string[], ...features: string[]): boolean`

Returns true if the given [browserslist](https://github.com/browserslist/browserslist) supports all of the given [caniuse](https://caniuse.com/) or [MDN](https://github.com/mdn/browser-compat-data) features

##### `browserslistSupportsEcmaVersion (browserslist: string[], version: EcmaVersion): boolean`

Returns true if the given [browserslist](https://github.com/browserslist/browserslist) supports the given ECMAScript version.
See [this section](#possible-ecmascript-versions) for an overview of all supported ECMAScript versions.

##### `getAppropriateEcmaVersionForBrowserslist (browserslist: string[]): EcmaVersion`

Gets the ECMAScript version that is most appropriate for the given [browserslist](https://github.com/browserslist/browserslist).
See [this section](#possible-ecmascript-versions) for an overview of all supported ECMAScript versions.

##### `matchBrowserslistOnUserAgent (userAgent: string, browserslist: string[]): boolean`

Will check if the given user agent string matches the given browserslist and return true if so.

##### `userAgentSupportsFeatures (useragent: string, ...features: string[]): boolean`

Checks if the given user agent string supports all of the given [caniuse](https://caniuse.com/) or [MDN](https://github.com/mdn/browser-compat-data) features.

##### `normalizeBrowserslist (browserslist: string[]|string): string[]`

Generates a normalized Browserslist from the given one.
