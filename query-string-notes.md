Based on this implementaiton - https://www.npmjs.com/package/query-string
we should make sure we support all common cases such as:
 * **bracket**: `'foo[]=1&foo[]=2&foo[]=3' => {foo: ['1', '2', '3']}`
 * **index**: `'foo[0]=1&foo[1]=2&foo[3]=3' => {foo: ['1', '2', '3']}`
 * **comma**: `'foo=1,2,3' => {foo: ['1', '2', '3']}`




## API

### .parse(string, options?)

Parse a query string into an object. Leading `?` or `#` are ignored, so you can pass `location.search` or `location.hash` directly.

The returned object is created with [`Object.create(null)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) and thus does not have a `prototype`.

#### options

Type: `object`

##### decode

Type: `boolean`
Default: `true`

Decode the keys and values. URL components are decoded with [`decode-uri-component`](https://github.com/SamVerschueren/decode-uri-component).

##### arrayFormat

Type: `string`
Default: `'none'`

- `'bracket'`: Parse arrays with bracket representation:

```js
import queryString from 'query-string';

queryString.parse('foo[]=1&foo[]=2&foo[]=3', {arrayFormat: 'bracket'});
//=> {foo: ['1', '2', '3']}
```

- `'index'`: Parse arrays with index representation:

```js
import queryString from 'query-string';

queryString.parse('foo[0]=1&foo[1]=2&foo[3]=3', {arrayFormat: 'index'});
//=> {foo: ['1', '2', '3']}
```

- `'comma'`: Parse arrays with elements separated by comma:

```js
import queryString from 'query-string';

queryString.parse('foo=1,2,3', {arrayFormat: 'comma'});
//=> {foo: ['1', '2', '3']}
```

- `'separator'`: Parse arrays with elements separated by a custom character:

```js
import queryString from 'query-string';

queryString.parse('foo=1|2|3', {arrayFormat: 'separator', arrayFormatSeparator: '|'});
//=> {foo: ['1', '2', '3']}
```

- `'bracket-separator'`: Parse arrays (that are explicitly marked with brackets) with elements separated by a custom character:

```js
import queryString from 'query-string';

queryString.parse('foo[]', {arrayFormat: 'bracket-separator', arrayFormatSeparator: '|'});
//=> {foo: []}

queryString.parse('foo[]=', {arrayFormat: 'bracket-separator', arrayFormatSeparator: '|'});
//=> {foo: ['']}

queryString.parse('foo[]=1', {arrayFormat: 'bracket-separator', arrayFormatSeparator: '|'});
//=> {foo: ['1']}

queryString.parse('foo[]=1|2|3', {arrayFormat: 'bracket-separator', arrayFormatSeparator: '|'});
//=> {foo: ['1', '2', '3']}

queryString.parse('foo[]=1||3|||6', {arrayFormat: 'bracket-separator', arrayFormatSeparator: '|'});
//=> {foo: ['1', '', 3, '', '', '6']}

queryString.parse('foo[]=1|2|3&bar=fluffy&baz[]=4', {arrayFormat: 'bracket-separator', arrayFormatSeparator: '|'});
//=> {foo: ['1', '2', '3'], bar: 'fluffy', baz:['4']}
```

- `'colon-list-separator'`: Parse arrays with parameter names that are explicitly marked with `:list`:

```js
import queryString from 'query-string';

queryString.parse('foo:list=one&foo:list=two', {arrayFormat: 'colon-list-separator'});
//=> {foo: ['one', 'two']}
```

- `'none'`: Parse arrays with elements using duplicate keys:

```js
import queryString from 'query-string';

queryString.parse('foo=1&foo=2&foo=3');
//=> {foo: ['1', '2', '3']}
```

