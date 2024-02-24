# get-sass-vars

[![Build Status][ci-img]][ci]

Get Sass variables as JSON object.

## Install

```sh
npm install get-sass-vars --save
```

## Usage

```js
import { promises as fs } from 'fs';
import sassVars from 'get-sass-vars';

(async () => {
	const css = await fs.readFile('./index.scss', 'utf-8');
	const json = await sassVars(css);
	console.log(json);
	/* {
		"$foo": "16px",
		"$bar": "17.6px",
		"$baz": 42,
		"$foo-bar": "#666",
		"$foo-bar-baz": "#262626",
		"$foo-bar-baz-bad": "#123",
		"$grault": [1, 2, "3", "4px", "42%", "1.23457px", [4, 5, 6], {"foo": "bar baz"}],
		"$garply": {"foo": 1, "bar": [2, 3], "baz": "3 3 3"},
		"$qux": false,
		"$fred": true,
		"$corgle": null
	} */
})();
```

### `index.scss`

```scss
$foo: 16px;
$bar: $foo * 1.1;
$baz: 42;
$foo-bar: #666;
$foo-bar-baz: darken($foo-bar, 25%);
$foo-bar-baz-bad: #123 !default;
$grault: 1, 2, '3', 4px, 42%, 1.23456789px, (4, 5, 6), (
		foo: 'bar baz'
	);
$garply: (
	foo: 1,
	bar: (
		2,
		3
	),
	baz: '3 3 3'
);
$qux: false;
$fred: true;
$corgle: null;

.nested {
	.selector {
		$nested-var: thud;
	}
}
```

## API

### sassVars(input[, options])

Returns: `Promise<JsonObject>`

Gets Sass variables from Sass string.

Only top-level variables will be considered, anything inside selector or at-rule
is ignored.

#### input

Type: `string`

Sass input string.

#### options

Type: `object`

##### camelize

Type: `boolean`  
Default: `false`

Camelize first-level JSON object keys and strip inital `$` (e.g. `$foo-bar` will
become `fooBar`).

##### sassOptions

Type: `sass.Options<"async">`  
Default: `{}`

[Options for Sass renderer][sass-options].

### sassVars.sync(input[, options])

Returns: `JsonObject`

Gets Sass variables from Sass string. Sync version.

All arguments are same as async version, except `options.sassOptions` which is
of type `sass.Options<"sync">`.

## Related

-   [get-sass-vars-loader][get-sass-vars-loader] - Webpack loader for this
    module

## License

MIT © [Ivan Nikolić](http://ivannikolic.com)

<!-- prettier-ignore-start -->

[ci]: https://github.com/niksy/get-sass-vars/actions?query=workflow%3ACI
[ci-img]: https://github.com/niksy/get-sass-vars/workflows/CI/badge.svg?branch=master
[sass-options]: https://sass-lang.com/documentation/js-api/interfaces/options/
[get-sass-vars-loader]: https://github.com/brianvoe/get-sass-vars-loader

<!-- prettier-ignore-end -->
