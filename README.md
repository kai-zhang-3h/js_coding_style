# JavaScript スタイルガイド

*現在、JavascriptとJSのフレームワークはかなり人気があります。ただし、資料は何千種類があって、明らかにJSの色々を説明できる参考資料は必要としています。それはこのJSスタイルガイドの目的です*

> **注意**: このガイドはJavascriptの初心者向けです。基本的に3H内部教育の資料としています。

## Table of Contents

  1. [Types](#types)
  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Arrow Functions](#arrow-functions)
  1. [Classes & Constructors](#classes--constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators-and-generators)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Control Statements](#control-statements)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Events](#events)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [ECMAScript 6+ (ES 2015+) Styles](#ecmascript-6-es-2015-styles)
  1. [Standard Library](#standard-library)

## Types

  <a name="types--primitives"></a><a name="1.1"></a>

  - [1.1](#types--primitives) **プリミティブ型**: プリミティブ型は、その値を直接操作します。
    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`
    - `symbol`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

    - Symbolは忠実にポリフィルされることはできないので、それらをネイティブにサポートしていないブラウザ/環境をターゲットにしているときは使用しない。

  <a name="types--complex"></a><a name="1.2"></a>

  - [1.2](#types--complex) **Complex**: When you access a complex type you work on a reference to its value.
  - [1.2](#types--complex) **参照型**: 参照型は、参照を通して値を操作します。

    - `object`
    - `array`
    - `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#table-of-contents)**

## References

  <a name="references--prefer-const"></a><a name="2.1"></a>

  - [2.1](#references--prefer-const) Use `const` for all of your references; avoid using `var`. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)
  - [2.1](#references--prefer-const) すべての参照は`const`を使用し、`var`を使用しない。eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

    > 原因： 参照を再割り当でできないことで、バグに繋がりやすく理解しがたいコードになることを防ぎます。

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="2.2"></a>

  - [2.2](#references--disallow-var) 参照を再割当てする場合は `var` の代わりに `let` を使用すること。eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

    > Why? `let` is block-scoped rather than function-scoped like `var`.

    > 原因： `let`は`var`のように関数スコープではなくブロックスコープです。

    ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  <a name="references--block-scope"></a><a name="2.3"></a>

  - [2.3](#references--block-scope) `let` と `const` は共にブロックスコープであることに注意すること。

    ```javascript
    // const and let only exist in the blocks they are defined in.
    // const と let はそれらが宣言されたブロックの中でのみ存在します。
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ back to top](#table-of-contents)**

## Objects

  <a name="objects--no-new"></a><a name="3.1"></a>
 
  - [3.1](#objects--no-new) オブジェクトを作成する際は、リテラル構文を使用すること。eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  <a name="es6-computed-properties"></a><a name="3.4"></a>
  
  - [3.2](#es6-computed-properties) 動的にプロパティ名を持つオブジェクトを作成する場合、計算されたプロパティ名(computed property names)を使用すること。

    > 原因： こうすることで、オブジェクトのプロパティを1箇所で定義することができます。

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a><a name="3.5"></a>

  - [3.3](#es6-object-shorthand) メソッドの短縮構文を使用すること。eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    ```javascript
    // bad
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // good
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a><a name="3.6"></a>

  - [3.4](#es6-object-concise) プロパティの短縮構文を使用すること。eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    > Why? 記述や説明が簡潔になるからです。

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.7"></a>

  - [3.5](#3.7) <a name='3.7'></a> プロパティの短縮構文はオブジェクト宣言の先頭にまとめること。

    > 原因： どのプロパティが短縮構文を利用しているか分かりやすいからです。

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.8"></a>

  - [3.6](#objects--quoted-props) 無効な識別子の場合のみプロパティを引用符で括ること。eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html)

    > 原因： 一般的にこちらの方が読みやすいと考えています。これは構文の強調表示を改善し、また多くのJSエンジンによってより簡単に最適化されます。

    ```javascript
    // bad
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // good
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--prototype-builtins"></a>

  - [3.7](#objects--prototype-builtins) `hasOwnProperty`、`propertyIsEnumerable`、`isPrototypeOf`のような`Object.prototype`の関数を直接呼び出さない。eslint: [`no-prototype-builtins`](https://eslint.org/docs/rules/no-prototype-builtins)

    > 原因： これらのメソッドはオブジェクトのプロパティによって隠されている可能性があるかもしれません - `{ hasOwnProperty：false }`のようなケースを考えてください - あるいは、オブジェクトはnullオブジェクト( `Object.create(null)` )になっているかもしれません。

    ```javascript
    // bad
    console.log(object.hasOwnProperty(key));

    // good
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // best
    const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
    /* or */
    import has from 'has'; // https://www.npmjs.com/package/has
    // ...
    console.log(has.call(object, key));
    ```

  <a name="objects--rest-spread"></a>

  - [3.8](#objects--rest-spread) オブジェクトをshallow-copyする場合は[`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)よりもオブジェクトスプレッド構文を使用すること。特定のプロパティを省略した新しいオブジェクトを取得するには、オブジェクトのレスト演算子を使用すること。

    ```javascript
    // very bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
    delete copy.a; // so does this

    // bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // good
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  <a name="arrays--literals"></a><a name="4.1"></a>

  - [4.1](#arrays--literals) 配列を作成する際は、リテラル構文を使用すること。eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>

  - [4.2](#arrays--push) 直接配列に項目を代入せず、[Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push)を使用すること。

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>

  - [4.3](#es6-array-spreads) 配列をコピーする場合は、配列の拡張演算子`...`を使用すること。

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a>
  <a name="arrays--from-iterable"></a><a name="4.4"></a>

  - [4.4](#arrays--from-iterable) 繰り返し可能なオブジェクトを配列に変換する場合は[`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from)の代わりにスプレッド構文`...`を使用すること。

    ```javascript
    const foo = document.querySelectorAll('.foo');

    // good
    const nodes = Array.from(foo);

    // best
    const nodes = [...foo];
    ```

  <a name="arrays--from-array-like"></a>
 
  - [4.5](#arrays--from-array-like) array-likeなオブジェクトを配列に変換する場合は[`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from)を使用すること。

    ```javascript
    const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

    // bad
    const arr = Array.prototype.slice.call(arrLike);

    // good
    const arr = Array.from(arrLike);
    ```

  <a name="arrays--mapping"></a>

  - [4.6](#arrays--mapping) 繰り返し可能なオブジェクト(iterables)へのマッピングにはスプレッド構文`...`の代わりに[`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from)を使用すること。理由は中間配列の作成を防ぐためです。

    ```javascript
    // bad
    const baz = [...foo].map(bar);

    // good
    const baz = Array.from(foo, bar);
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>

  - [4.7](#arrays--callback-return) 配列のコールバック関数の中ではreturn構文を使用すること。もし関数の中が副作用のない式を返す一行で構成されている場合はreturnを省略してもかまいません。後述 [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map(x => x + 1);

    // bad - no returned value means `acc` becomes undefined after the first iteration
    // bad - 値を返さないために`acc`は最初の繰り返しのあとでundefinedになります。
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      acc[index] = flatten;
    });

    // good
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      acc[index] = flatten;
      return flatten;
    });

    // bad
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // good
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a>

  <a name="arrays--bracket-newline"></a>
  - [4.8](#arrays--bracket-newline) 配列が複数行ある場合、配列の開始の括弧の後と、最後の括弧の前に改行を入れること。

    ```javascript
    // bad
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    const numberInArray = [
      1, 2,
    ];

    // good
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const numberInArray = [
      1,
      2,
    ];
    ```

**[⬆ back to top](#table-of-contents)**

## Destructuring

  <a name="destructuring--object"></a><a name="5.1"></a>

  - [5.1](#destructuring--object) 複数のプロパティからなるオブジェクトにアクセスする際は、オブジェクト構造化代入を使用すること。eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    > 原因： 構造化代入を利用することで、それらのプロパティのための中間的な参照を減らすことができます。

    ```javascript
    // bad
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(obj) {
      const { firstName, lastName } = obj;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  <a name="destructuring--array"></a><a name="5.2"></a>
  
  - [5.2](#destructuring--array) 配列の構造化代入を使用すること。eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  <a name="destructuring--object-over-array"></a><a name="5.3"></a>

  - [5.3](#destructuring--object-over-array) 複数の値を返却する場合は、配列の構造化代入ではなく、オブジェクトの構造化代入を使用すること。

    > 原因： こうすることで、後で新しいプロパティを追加したり、呼び出し元に影響することなく順序を変更することができます。

    ```javascript
    // bad
    function processInput(input) {
      // then a miracle occurs
      // その後、奇跡が起こります。
      return [left, right, top, bottom];
    }

    // the caller needs to think about the order of return data
    // 呼び出し者で返却されるデータの順番を考慮する必要があります。
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
      // then a miracle occurs
      // その後、奇跡が起こります。
      return { left, right, top, bottom };
    }

    // the caller selects only the data they need
    // 呼び出し元は必要なデータのみ選択すればいい。
    const { left, top } = processInput(input);
    ```

**[⬆ back to top](#table-of-contents)**

## Strings

  <a name="strings--quotes"></a><a name="6.1"></a>

  - [6.1](#strings--quotes) 文字列にはシングルクオート `''` を使用すること。eslint: [`quotes`](https://eslint.org/docs/rules/quotes.html)

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // bad - template literals should contain interpolation or newlines
    const name = `Capt. Janeway`;

    // good
    const name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>

  - [6.2](#strings--line-length) 100文字以上になるような文字列は、文字列連結を使用して複数行にまたがって記述しない。

    > 原因： 壊れた文字列は作業する上で辛すぎます、さらにコードの検索容易性を損ないます。

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.4"></a>

  - [6.3](#es6-template-literals) プログラムで文字列を生成する場合は、文字列連結ではなく、template stringsを使用すること。eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    > 原因： Template strings は文字列補完機能・複数行文字列機能を持つ簡潔な構文で、可読性が良いからです。

    ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // bad
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

  <a name="strings--eval"></a><a name="6.5"></a>

  - [6.4](#strings--eval) 絶対に `eval()` を利用しない。これは、いままで数多くの脆弱性を作って来たからです。eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

  <a name="strings--escaping"></a>

  - [6.5](#strings--escaping) 文字列の中で文字を不必要にエスケープしない。eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

    > 原因： バックスラッシュは可読性を損なうため、必要なときにだけ存在させるべきです。

    ```javascript
    // bad
    const foo = '\'this\' \i\s \"quoted\"';

    // good
    const foo = '\'this\' is "quoted"';
    const foo = `my name is '${name}'`;
    ```

**[⬆ back to top](#table-of-contents)**

## Functions

  <a name="functions--declarations"></a><a name="7.1"></a>

  - [7.1](#functions--declarations) 関数宣言ではなく名前付き関数式を使用すること。eslint: [`func-style`](https://eslint.org/docs/rules/func-style)

    > 原因： 関数の定義は巻き上げられます。このことはそれがファイルの中で定義されている場所よりも前に簡単に - そうとても簡単に - 参照されることを意味します。それは読みやすさや保守性を損ないます。関数の定義がファイルの残りの部分を理解するのを妨げているくらい十分に大きいか複雑であることがわかった場合、おそらくそれをモジュールに展開する時が来たという事でしょう！名前が定義さてている変数から推測されるかどうかにかかわらず、式に明示的に名前を付けることを忘れないでください(モダンブラウザやBabelなどのコンパイラを使用している場合によくあります)。これにより、エラーの呼び出しスタックに関して行われた前提がなくなります。

    ```javascript
    // bad
    function foo() {
      // ...
    }

    // bad
    const foo = function () {
      // ...
    };

    // good
    // lexical name distinguished from the variable-referenced invocation(s)
    const short = function longUniqueMoreDescriptiveLexicalFoo() {
      // ...
    };
    ```
  <a name="functions--iife"></a><a name="7.2"></a>

  - [7.2](#functions--iife) 即時呼び出し関数式(IIFE)を括弧で囲むこと。eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife.html)

    > 原因： 即時関数式は単一の単位であり、その両方とその呼び出し括弧を括弧で囲んで明確に表します。注意：モジュールがある世界では、IIFEはほとんど必要ないことを留意してください。

    ```javascript
    // immediately-invoked function expression (IIFE)
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  <a name="functions--in-blocks"></a><a name="7.3"></a>

  - [7.3](#functions--in-blocks) 関数ではないブロック(`if`, `while`, など)の中で関数を定義しない。代わりに変数に関数を割り当てること。ブラウザはそのことを許可しますが、（それはまるで「頑張れベアーズ」の悪ガキ達のように）すべて違ったように解釈されます。eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func.html)

  <a name="functions--note-on-blocks"></a><a name="7.4"></a>

  - [7.4](#functions--note-on-blocks) **注意:** ECMA-262仕様では `block` はstatementsの一覧に定義されていますが、関数宣言はstatementsではありません。

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="7.5"></a>

  - [7.5](#functions--arguments-shadow) パラメータに `arguments` を指定しない。これは、関数スコープに渡される `arguments` オブジェクトの参照を上書きしてしまうためです。

    ```javascript
    // bad
    function foo(name, options, arguments) {
      // ...
    }

    // good
    function foo(name, options, args) {
      // ...
    }
    ```

  <a name="es6-rest"></a><a name="7.6"></a>
 
  - [7.6](#es6-rest) `arguments` を利用しない。代わりにrest syntax `...` を使用すること。eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

    > 原因： `...` を利用することで、いつくかのパラメータを利用したいことを明らかにすることができます。加えてrestパラメータは `arguments` の様なArray-likeなオブジェクトではなく正真正銘のArrayです。

    ```javascript
    // bad
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // good
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a><a name="7.7"></a>

  - [7.7](#es6-default-parameters) 関数のパラメータを突然変異させるのではなく、デフォルトパラメータを使用すること。

    ```javascript
    // really bad
    function handleThings(opts) {
      // No! We shouldn't mutate function arguments.
      // Double bad: if opts is falsy it'll be set to an object which may
      // be what you want but it can introduce subtle bugs.

      // だめ！関数のパラメータを突然変異させるべきではありません。
      // もし、optsがfalsyだった場合は、望んだようにオブジェクトが設定されます。
      // しかし、微妙なバグを引き起こすかもしれません。
      opts = opts || {};
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```

  <a name="functions--default-side-effects"></a><a name="7.8"></a>

  - [7.8](#functions--default-side-effects) 副作用のあるデフォルトパラメータの利用を避ける。

    > 原因： 混乱させるからです。

    ```javascript
    var b = 1;
    // bad
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a><a name="7.9"></a>

  - [7.9](#functions--defaults-last) 常にデフォルトパラメータは末尾に配置すること。

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
      // ...
    }

    // good
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  <a name="functions--constructor"></a><a name="7.10"></a>
 
  - [7.10](#functions--constructor) 新しい関数を作成するためにFunctionコンストラクタを使用しない。 eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

    > 原因： この方法で文字列を評価させて新しい関数を作成することは、eval()と同様の脆弱性を引き起こすことになります。

    ```javascript
    // bad
    var add = new Function('a', 'b', 'return a + b');

    // still bad
    var subtract = Function('a', 'b', 'return a - b');
    ```

<a name="functions--signature-spacing"></a><a name="7.11"></a>

  - [7.11](#functions--signature-spacing) 関数構文の中にスペースを入れること。eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    > 原因： 一貫性は良い事であり、名前を追加または削除するときにスペースを追加または削除する必要はありません。

    ```javascript
    // bad
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // good
    const x = function () {};
    const y = function a() {};
    ```

  <a name="functions--mutate-params"></a><a name="7.12"></a>
 
  - [7.12](#functions--mutate-params) パラメータを直接操作しないこと。eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > 原因： パラメータに渡されたオブジェクトを操作することは、呼び出し元にて望まれない値の副作用を及ぼす可能性があります。

    ```javascript
    // bad
    function f1(obj) {
      obj.key = 1;
    }

    // good
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    }
    ```

  <a name="functions--reassign-params"></a><a name="7.13"></a>

  - [7.13](#functions--reassign-params) パラメータを再割り当てしない。eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > 原因： 特に `arguments`オブジェクトにアクセスするとき、パラメータを再割り当てすると予期しない動作をする可能性があります。 特にV8では最適化の問題も発生する可能性があります。

    ```javascript
    // bad
    function f1(a) {
      a = 1;
      // ...
    }

    function f2(a) {
      if (!a) { a = 1; }
      // ...
    }

    // good
    function f3(a) {
      const b = a || 1;
      // ...
    }

    function f4(a = 1) {
      // ...
    }
    ```

  <a name="functions--spread-vs-apply"></a><a name="7.14"></a>

  - [7.14](#functions--spread-vs-apply) 可変引数の関数を呼び出す場合はスプレッド演算子 `...` を使用すること。eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

    > 原因： それは分かりやすいです。コンテキストを提供する必要はありませんし、そして簡単に`apply`で`new`を構成することはできません。

    ```javascript
    // bad
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // good
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // bad
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

    // good
    new Date(...[2016, 8, 5]);
    ```

  <a name="functions--signature-invocation-indentation"></a>

  - [7.15](#functions--signature-invocation-indentation) 複数行の関数構文や呼び出しでは、このガイドの他のすべての複数行リストと同じようにインデントする必要があります。最後の項目に末尾のコンマを付けて、行の各項目を単独で指定すること。eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

    ```javascript
    // bad
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // good
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // bad
    console.log(foo,
      bar,
      baz);

    // good
    console.log(
      foo,
      bar,
      baz,
    );
    ```

**[⬆ back to top](#table-of-contents)**

## アロー関数(Arrow Functions)

  <a name="arrows--use-them"></a><a name="8.1"></a>

  - [8.1](#arrows--use-them)（インラインコールバックを渡すときのように）無名関数を使用する必要がある場合は、アロー関数表記を使用してください。eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing.html)

    > 原因： アロー関数はそのコンテキストの `this` で実行するバージョンの関数を作成します。これは通常期待通りの動作をし、より簡潔な構文だからです。

    > 利用するべきではない？ 複雑な関数でロジックを定義した関数の外側に移動したいケース。

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--implicit-return"></a><a name="8.2"></a>

  - [8.2](#arrows--implicit-return) 関数本体が副作用のない式を返す単一の文で構成されている場合は、中括弧を省略して暗黙の戻り値を使用すること。 そうでなければ、中括弧を付けて`return`文を使うこと。 eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

    > 原因： シンタックスシュガー(糖衣構文)。それは複数の関数が連結されている場合に読み易くなります。

    ```javascript
    // bad
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // good
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // No implicit return with side effects
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // Do something if callback returns true
      }
    }

    let bool = false;

    // bad
    foo(() => bool = true);

    // good
    foo(() => {
      bool = true;
    });
    ```

  - [8.3](#8.3) <a name='8.3'></a> 式の全長が複数行にまたがる場合は、可読性をより良くするため丸括弧()で囲うこと。

    > 原因： 関数の開始と終了部分が分かりやすく見えるため。

    ```javascript
    // bad
    ['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    );

    // good
    ['get', 'post', 'put'].map(httpMethod => (
      Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    ));
    ```

  <a name="arrows--one-arg-parens"></a><a name="8.4"></a>

  - [8.4](#arrows--one-arg-parens) 関数が単一の引数を取り、中括弧を使用しない場合は、括弧を省略すること。 それ以外の場合は、明確さと一貫性を保つために、引数を常に括弧で囲むこと。 注意：常に括弧を使用することもできます。その場合は、eslintに["always"オプション](https://eslint.org/docs/rules/arrow-parens#always)を使用すること。eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html)

    > 原因： あまり見難くないからです。

    ```javascript
    // bad
    [1, 2, 3].map((x) => x * x);

    // good
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // bad
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--confusing"></a><a name="8.5"></a>

  - [8.5](#arrows--confusing) アロー関数の構文(`=>`)と比較演算子(`<=`、`>=`)を混同しない。eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)
    ```javascript
    // bad
    const itemHeight = item => item.height <= 256 ? item.largeSize : item.smallSize;

    // bad
    const itemHeight = (item) => item.height >= 256 ? item.largeSize : item.smallSize;

    // good
    const itemHeight = item => (item.height <= 256 ? item.largeSize : item.smallSize);

    // good
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height <= 256 ? largeSize : smallSize;
    };
    ```

  <a name="whitespace--implicit-arrow-linebreak"></a>

  - [8.6](#whitespace--implicit-arrow-linebreak) 暗黙的な戻り値でアロー関数本体の位置を強制すること。eslint: [`implicit-arrow-linebreak`](https://eslint.org/docs/rules/implicit-arrow-linebreak)

    ```javascript
    // bad
    foo =>
      bar;

    foo =>
      (bar);

    // good
    foo => bar;
    foo => (bar);
    foo => (
       bar
    )
    ```

**[⬆ back to top](#table-of-contents)**

## Classes & Constructors

  <a name="constructors--use-class"></a><a name="9.1"></a>

  - [9.1](#constructors--use-class) `prototype` を直接操作することを避け、常に `class` を使用すること。

    > 原因： `class` 構文は簡潔で意図がわかりやすいから。

    ```javascript
    // bad
    function Queue(contents = []) {
      this.queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    };

    // good
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents];
      }
      pop() {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
      }
    }
    ```

  <a name="constructors--extends"></a><a name="9.2"></a>
  - [9.2](#constructors--extends) Use `extends` for inheritance.
  - [9.2](#constructors--extends) 継承には`extends`を使用すること。

    > 原因： これは`instanceof`を破壊することなくプロトタイプ継承するためのビルトインされた方法です。

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this.queue[0];
    };

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0];
      }
    }
    ```

  <a name="constructors--chaining"></a><a name="9.3"></a>

  - [9.3](#constructors--chaining) メソッドの戻り値で`this`を返すことでメソッドチェーンを助けること。

    ```javascript
    // bad
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  <a name="constructors--tostring"></a><a name="9.4"></a>

  - [9.4](#constructors--tostring) 独自の`toString()`を作成することも認めますが、正しく動作することと副作用がないことを確認すること。

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>

  - [9.5](#constructors--no-useless) 一つも指定されていない場合、クラスにはデフォルトのコンストラクタがあります。空のコンストラクタ関数やただ親クラスに移譲するだけのものは不要です。eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // bad
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // bad
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // good
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  <a name="classes--no-duplicate-members"></a>

  - [9.6](#classes--no-duplicate-members) クラスのメンバの重複を避ける。 eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > 原因： 重複したクラスメンバの宣言は暗黙的に最後のものが適用されます。 - 重複を持つことはほぼ確実にバグです。

    ```javascript
    // bad
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // good
    class Foo {
      bar() { return 1; }
    }

    // good
    class Foo {
      bar() { return 2; }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Modules
  <a name="modules--use-them"></a><a name="10.1"></a>

  - [10.1](#modules--use-them) 非標準モジュールシステム上では常にモジュール（`import`/`export`）を使用すること。いつもあなたの好みのモジュールシステムにトランスパイルすることができます。

    > 原因： モジュールは未来です。今から未来を先取りして使い始めましょう。

    ```javascript
    // bad
    const FooStyleGuide = require('./FooStyleGuide');
    module.exports = FooStyleGuide.es6;

    // ok
    import FooStyleGuide from './FooStyleGuide';
    export default FooStyleGuide.es6;

    // best
    import { es6 } from './FooStyleGuide';
    export default es6;
    ```

  <a name="modules--no-wildcard"></a><a name="10.2"></a>

  - [10.2](#modules--no-wildcard) ワイルドカードインポートは使用しない。

    > 原因： default exportが一つであることを注意する必要があるからです。

    ```javascript
    // bad
    import * as FooStyleGuide from './FooStyleGuide';

    // good
    import FooStyleGuide from './FooStyleGuide';
    ```

  <a name="modules--no-export-from-import"></a><a name="10.3"></a>

  - [10.3](#modules--no-export-from-import) import文から直接exportしない。

    > 原因： ワンライナーは簡潔ですが、インポートする1つの明確な方法とエクスポートする1つの明確な方法を持つことは物事を一貫性のあるものにします。

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './FooStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './FooStyleGuide';
    export default es6;
    ```

  <a name="modules--no-duplicate-imports"></a>

  - [10.4](#modules--no-duplicate-imports) パスからは一箇所のみでインポートすること。eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)

    > 原因： 同じパスからインポートする複数の行があると、コードが保守しにくくなります。

    ```javascript
    // bad
    import foo from 'foo';
    // … some other imports … //
    import { named1, named2 } from 'foo';

    // good
    import foo, { named1, named2 } from 'foo';

    // good
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

  <a name="modules--no-mutable-exports"></a>

  - [10.5](#modules--no-mutable-exports) 可変のバインディングをエクスポートしない。eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)

    > 原因： 突然変異は一般的には避けるべきですが、特に可変のバインディングをエクスポートすることは避けてください。この手法はいくつかの特別な場合に必要になるかもしれませんが、一般的には定数参照だけをエクスポートするべきです。

    ```javascript
    // bad
    let foo = 3;
    export { foo };

    // good
    const foo = 3;
    export { foo };
    ```

  <a name="modules--prefer-default-export"></a>

  - [10.6](#modules--prefer-default-export) 単一のエクスポートを持つモジュールでは、名前付きエクスポートよりもデフォルトエクスポートすること。
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)


    > 原因： 1つだけをエクスポートするファイルを増やすことをお勧めします。これは、読みやすさと保守性の点で優れています。

    ```javascript
    // bad
    export function foo() {}

    // good
    export default function foo() {}
    ```

  <a name="modules--imports-first"></a>

  - [10.7](#modules--imports-first) すべての`import`をimport文以外の上に置くこと。
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)

    > 原因： `import`は巻き上げられているので、それらすべてを一番上にしておくことは驚くべき振る舞いが起こるのを防ぎます。

    ```javascript
    // bad
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // good
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

  <a name="modules--multiline-imports-over-newlines"></a>

  - [10.8](#modules--multiline-imports-over-newlines) 複数行のインポートは、複数行の配列リテラルおよびオブジェクトリテラルと同じようにインデントすること。

    > 原因： 中括弧は、末尾のコンマと同様に、スタイルガイド内の他のすべての中括弧ブロックと同じインデント規則に従います。

    ```javascript
    // bad
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // good
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path';
    ```

  <a name="modules--no-webpack-loader-syntax"></a>

  - [10.9](#modules--no-webpack-loader-syntax) モジュールのインポート文でWebpackローダーの構文を許可しない。
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)

    > 原因： インポートでWebpack構文を使用することは、コードをモジュールバンドラーに結び付けてしまいます。`webpack.config.js`の中でローダー構文を使うようにしてください。

    ```javascript
    // bad
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // good
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

**[⬆ back to top](#table-of-contents)**

## Iterators and Generators

  <a name="iterators--nope"></a><a name="11.1"></a>

  - [11.1](#iterators--nope) イテレータを使わない。`for-in`や`for-of`のようなループの代わりにJavaScriptの高階関数(higher-order functions)を使用すること。

    > 原因： これは私たちの不変(immutable)のルールを強制します。値を返す純粋な関数を扱うことは、副作用よりも簡単に推論できます。

    > 配列を反復処理するには `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... を使用すること。そして`Object.keys()` / `Object.values()` / `Object.entries()` で配列を作成してオブジェクトを反復処理できるようにすること。

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }
    sum === 15;

    // good
    let sum = 0;
    numbers.forEach((num) => {
      sum += num;
    });
    sum === 15;

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;

    // bad
    const increasedByOne = [];
    for (let i = 0; i < numbers.length; i++) {
      increasedByOne.push(numbers[i] + 1);
    }

    // good
    const increasedByOne = [];
    numbers.forEach((num) => {
      increasedByOne.push(num + 1);
    });

    // best (keeping it functional)
    const increasedByOne = numbers.map(num => num + 1);
    ```

  <a name="generators--nope"></a><a name="11.2"></a>

  - [11.2](#generators--nope) 今のところgeneratorsは使用しない。

    > Why? They don’t transpile well to ES5.

    > Why? ES5にうまくトランスパイルできないから。

  <a name="generators--spacing"></a>

  - [11.3](#generators--spacing) ジェネレータを使用する必要がある場合、また[われわれのアドバイス](#generators--nope)を無視する場合は、それらの関数シグネチャが適切に配置されていることを確認すること。eslint: [`generator-star-spacing`](https://eslint.org/docs/rules/generator-star-spacing)

    > 原因： `function`と`*`は同じ概念的なキーワードの一部です - `*`は`function`の修飾子ではなく、`function*`は`function`とは異なるユニークな構成要素です。

    ```javascript
    // bad
    function * foo() {
      // ...
    }

    // bad
    const bar = function * () {
      // ...
    };

    // bad
    const baz = function *() {
      // ...
    };

    // bad
    const quux = function*() {
      // ...
    };

    // bad
    function*foo() {
      // ...
    }

    // bad
    function *foo() {
      // ...
    }

    // very bad
    function
    *
    foo() {
      // ...
    }

    // very bad
    const wat = function
    *
    () {
      // ...
    };

    // good
    function* foo() {
      // ...
    }

    // good
    const foo = function* () {
      // ...
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Properties

  <a name="properties--dot"></a><a name="12.1"></a>

  - [12.1](#properties--dot) プロパティにアクセスする場合はドット(`.`)を使用すること。eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // bad
    const isJedi = luke['jedi'];

    // good
    const isJedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="12.2"></a>

  - [12.2](#properties--bracket) 変数を使用してプロパティにアクセスする場合は角括弧(`[]`)を使用すること。

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

  <a name="es2016-properties--exponentiation-operator"></a>

  - [12.3](#es2016-properties--exponentiation-operator) べき乗を計算するときは、べき乗演算子`**`を使用すること。eslint: [`no-restricted-properties`]

    ```javascript
    // bad
    const binary = Math.pow(2, 10);

    // good
    const binary = 2 ** 10;
    ```

**[⬆ back to top](#table-of-contents)**

## Variables

  <a name="variables--const"></a><a name="13.1"></a>

  - [13.1](#variables--const) 変数を宣言する際は、常に`const`か`let`を使用すること。使用しない場合はグローバル変数として宣言されてしまいます。グローバルな名前空間を汚染しないように、キャプテンプラネット（環境保護とエコロジーをテーマにしたスーパーヒーローアニメ）も警告しています。eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  <a name="variables--one-const"></a><a name="13.2"></a>
 
  - [13.2](#variables--one-const) 1つの変数宣言に対して1つの`const`を利用すること。eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)

    > 原因： この方法の場合、簡単に新しい変数を追加することができます。また、二度と区切り文字の違いによる`;`を`,`に置き換える作業を心配することがありません。すべての宣言を一度にジャンプするのではなく、デバッガーを使用して各宣言をステップスルーすることもできます。

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  <a name="variables--const-let-group"></a><a name="13.3"></a>

  - [13.3](#variables--const-let-group) まず`const`をグループ化し、その後 `let`をグループ化すること。

    > 原因： これは、以前に割り当てられた変数に後で変数を割り当てる必要がある場合に役立ちます。

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  <a name="variables--define-where-used"></a><a name="13.4"></a>

  - [13.4](#variables--define-where-used) 変数を割り当てる際は、必要かつ合理的な場所で行うこと。

    > 原因： `let`と`const`はブロックスコープだからです。関数スコープではありません。

    ```javascript
    // bad - unnecessary function call
    function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
        return false;
      }

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

    // good
    function checkName(hasName) {
      if (hasName === 'test') {
        return false;
      }

      const name = getName();

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }
    ```

  <a name="variables--no-chain-assignment"></a><a name="13.5"></a>

  - [13.5](#variables--no-chain-assignment) 変数代入を連結しない。eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

    > 原因： 変数代入を連鎖させると、暗黙のグローバル変数が作成されます。

    ```javascript
    // bad
    (function example() {
      // JavaScript interprets this as
      // let a = ( b = ( c = 1 ) );
      // The let keyword only applies to variable a; variables b and c become
      // global variables.

      // JavaScriptはこれを次のように解釈します
      // let a = ( b = ( c = 1 ) );
      // letキーワードは変数aにのみ適用されます。変数bとcはグローバル変数になります。
      let a = b = c = 1;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // 1
    console.log(c); // 1

    // good
    (function example() {
      let a = 1;
      let b = a;
      let c = a;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // throws ReferenceError
    console.log(c); // throws ReferenceError

    // the same applies for `const`
    ```

  <a name="variables--unary-increment-decrement"></a><a name="13.6"></a>

  - [13.6](#variables--unary-increment-decrement) インクリメント演算子(`++`)デクリメント演算子(`--`)を使わない。eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

    > 原因： eslintのドキュメントによると、インクリメントおよびデクリメント演算子は自動セミコロン挿入の対象となり、アプリケーション内で値の増加または減少を伴う予期せぬエラーを引き起こす可能性があります。`num++`や`num ++`の代わりに`num += 1`のような構文で値を変更する方が表現力豊かです。インクリメントおよびデクリメント演算子を許可しないことで、直前の意図しない値の増加または減少による、プログラムでの予期しない動作を引き起こす可能性を防ぐことができます。

    ```javascript
    // bad

    const array = [1, 2, 3];
    let num = 1;
    num++;
    --num;

    let sum = 0;
    let truthyCount = 0;
    for (let i = 0; i < array.length; i++) {
      let value = array[i];
      sum += value;
      if (value) {
        truthyCount++;
      }
    }

    // good

    const array = [1, 2, 3];
    let num = 1;
    num += 1;
    num -= 1;

    const sum = array.reduce((a, b) => a + b, 0);
    const truthyCount = array.filter(Boolean).length;
    ```

<a name="variables--linebreak"></a>

  - [13.7](#variables--linebreak) 代入で`=`の前後に改行を入れない。あなたの代入が[`max-len`](https://eslint.org/docs/rules/max-len.html)に違反している場合は、値を丸括弧()で囲むこと。 eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html)

    > 原因： `=`を囲む改行は代入の値を読みにくくすることがあります。

    ```javascript
    // bad
    const foo =
      superLongLongLongLongLongLongLongLongFunctionName();

    // bad
    const foo
      = 'superLongLongLongLongLongLongLongLongString';

    // good
    const foo = (
      superLongLongLongLongLongLongLongLongFunctionName()
    );

    // good
    const foo = 'superLongLongLongLongLongLongLongLongString';
    ```

<a name="variables--no-unused-vars"></a>

  - [13.8](#variables--no-unused-vars) 未使用の変数を許可しない。eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

    > 原因： 宣言されていてコードのどこにも使用されていない変数は、不完全なリファクタリングによる失敗である可能性が最も高いです。このような変数はコード内のスペースを占有し、他の開発者にとって混乱を招く可能性があります。

    ```javascript
    // bad

    var some_unused_var = 42;

    // Write-only variables are not considered as used.
    // 書き込み専用変数は、使用されているとは見なされません。
    var y = 10;
    y = 5;

    // A read for a modification of itself is not considered as used.
    // それ自体の変更のための読み取りは、使用済みとは見なされません。
    var z = 0;
    z = z + 1;

    // Unused function arguments.
    // 未使用の関数の引数。
    function getX(x, y) {
        return x;
    }

    // good

    function getXPlusY(x, y) {
      return x + y;
    }

    var x = 1;
    var y = a + 2;

    alert(getXPlusY(x, y));

    // 'type' is ignored even if unused because it has a rest property sibling.
    // This is a form of extracting an object that omits the specified keys.
    // 残りのプロパティがあるため、'type' は未使用でも無視されます。
    // これは、指定されたキーを省略したオブジェクトを抽出する形式です。
    var { type, ...coords } = data;
    // 'coords' is now the 'data' object without its 'type' property.
    // 'coords'は、今や'type'プロパティを持たない'data'オブジェクトになりました。
    ```

**[⬆ back to top](#table-of-contents)**

## Hoisting

  <a name="hoisting--about"></a><a name="14.1"></a>

  - [14.1](#hoisting--about) `var`宣言は、それらに最も近い包含関数スコープの一番上に引き上げられますが、それらの代入は引き継がれません。`const`と`let`の宣言は[Temporal Dead Zones(TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_Dead_Zone)と呼ばれる新しい概念の恩恵を受けています。[なぜtypeofが安全ではなくなったのか](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)を知ることは重要です。

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    // （notDefinedがグローバル変数に存在しないと仮定した場合。）
    // これはうまく動作しません。
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    // その変数を参照するコードの後でその変数を宣言した場合、
    // 変数が巻上げられた上で動作します。
    // 注意：`true` という値自体は巻き上げられません。
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    // インタープリンタは変数宣言をスコープの先頭に巻き上げます。
    // 上の例は次のように書き直すことが出来ます。
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // using const and let
    // const と let を利用した場合
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  <a name="hoisting--anon-expressions"></a><a name="14.2"></a>

  - [14.2](#hoisting--anon-expressions) 無名関数の場合、関数が割当てされる前の変数が巻き上げられます。

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  <a name="hoisting--named-expresions"></a><a name="hoisting--named-expressions"></a><a name="14.3"></a>

  - [14.3](#hoisting--named-expressions) 名前付き関数の場合も同様に変数が巻き上げられます。関数名や関数本体は巻き上げられません。

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    // 関数名と変数名が同じ場合も同じことが起きます。
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  <a name="hoisting--declarations"></a><a name="14.4"></a>
  
  - [14.4](#hoisting--declarations) 関数宣言は関数名と関数本体が巻き上げられます。

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 詳しくはこちらを参照してください。 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ back to top](#table-of-contents)**

## Comparison Operators & Equality

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>

  - [15.1](#comparison--eqeqeq) `==`や`!=`より`===`と`!==`を使用すること。eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

  <a name="comparison--if"></a><a name="15.2"></a>

  - [15.2](#comparison--if) `if`文のような条件式は`ToBoolean`メソッドによる強制型変換で評価され、常にこれらのシンプルなルールに従います。
    - **オブジェクト**は**true**と評価されます。
    - **undefined**は**false**と評価されます。
    - **null**は**false**と評価されます。
    - **真偽値**は**boolean型の値**として評価されます。
    - **数値**は**true**と評価されます。しかし、**+0, -0, or NaN**の場合は **false**です。
    - **文字列**は**true**と評価されます。しかし、空文字`''`の場合は**false** です。

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
      // 配列はオブジェクトなのでtrueとして評価されます。
    }
    ```

  <a name="comparison--shortcuts"></a><a name="15.3"></a>

  - [15.3](#comparison--shortcuts) 真偽値にはショートカットを使用しますが、文字列と数値には明示的な比較を使用すること。

    ```javascript
    // bad
    if (isValid === true) {
      // ...
    }

    // good
    if (isValid) {
      // ...
    }

    // bad
    if (name) {
      // ...
    }

    // good
    if (name !== '') {
      // ...
    }

    // bad
    if (collection.length) {
      // ...
    }

    // good
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--moreinfo"></a><a name="15.4"></a>

  - [15.4](#comparison--moreinfo) 詳しくはこちらを参照してください。[Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

  <a name="comparison--switch-blocks"></a><a name="15.5"></a>
  
  - [15.5](#comparison--switch-blocks) `case`と`default`節でブロックを作成するには中括弧(`{}`)を使うこと。(例えば `let`、`const`、`function`、`class`) eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html)

    > 原因： レキシカル宣言は`switch`ブロック全体から参照することができますが、代入されたときにだけ初期化され、それは`case`に達したときにだけ起こります。複数の`case`節が同じものを定義しようとするとき、これは問題を引き起こします。

    ```javascript
    // bad
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {
          // ...
        }
        break;
      default:
        class C {}
    }

    // good
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {
          // ...
        }
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.6"></a>

  - [15.6](#comparison--nested-ternaries)三項演算子は入れ子にするべきではなく、一般に単一行にします。eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html)

    ```javascript
    // bad
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // split into 2 separated ternary expressions
    const maybeNull = value1 > value2 ? 'baz' : null;

    // better
    const foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull;

    // best
    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.7"></a>

  - [15.7](#comparison--unneeded-ternary) 不要な3項ステートメントを避ける。eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html)

    ```javascript
    // bad
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;

    // good
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    ```

  <a name="comparison--no-mixed-operators"></a>

  - [15.8](#comparison--no-mixed-operators) 演算子を混在させる場合は、それらを括弧で囲むこと。唯一の例外は、標準の算術演算子（`+`、`-`、`*`、`/`）です。それらの優先順位は広く理解されているからです。eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

    > 原因： これにより読みやすさが向上し、開発者の意図が明確になります。

    ```javascript
    // bad
    const foo = a && b < 0 || c > 0 || d + 1 === 0;

    // bad
    const bar = a ** b - 5 % d;

    // bad
    // one may be confused into thinking (a || b) && c
    // (a || b) && c だと考えて混乱するかもしれません。
    if (a || b && c) {
      return d;
    }

    // good
    const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

    // good
    const bar = (a ** b) - (5 % d);

    // good
    if (a || (b && c)) {
      return d;
    }

    // good
    const bar = a + b / c * d;
    ```

**[⬆ back to top](#table-of-contents)**

## Blocks

  <a name="blocks--braces"></a><a name="16.1"></a>

  - [16.1](#blocks--braces) 複数行のブロックには中括弧（`{}`）を使用すること。eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function foo() { return false; }

    // good
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>

  - [16.2](#blocks--cuddled-elses) `if`と`else`を使った複数行のブロックの場合は、`else`ブロックの閉じ括弧と同じ行に`else`を置くこと。eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

  <a name="blocks--no-else-return"></a><a name="16.3"></a>

  - [16.3](#blocks--no-else-return) `if`ブロックが常に`return`文を実行するのであれば、それに続く`else`ブロックは不要です。`return`を含む`if`ブロックに続く `else if`ブロック内の`return`は複数の`if`ブロックに分けることができます。eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

    ```javascript
    // bad
    function foo() {
      if (x) {
        return x;
      } else {
        return y;
      }
    }

    // bad
    function cats() {
      if (x) {
        return x;
      } else if (y) {
        return y;
      }
    }

    // bad
    function dogs() {
      if (x) {
        return x;
      } else {
        if (y) {
          return y;
        }
      }
    }

    // good
    function foo() {
      if (x) {
        return x;
      }

      return y;
    }

    // good
    function cats() {
      if (x) {
        return x;
      }

      if (y) {
        return y;
      }
    }

    // good
    function dogs(x) {
      if (x) {
        if (z) {
          return y;
        }
      } else {
        return z;
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Control Statements

  <a name="control-statements"></a>

  <a name="control-statements"></a>

  - [17.1](#control-statements) 制御文（`if`、`while`など）が長くなり過ぎたり、最大行長を超えたりした場合、それぞれの（グループ化された）条件は新しい行に入れること。そして論理演算子で行を始めること。

    > 原因： 行の先頭で演算子を要求すると、演算子は整列されたままになり、メソッド連結と同様のパターンに従います。これにより、複雑なロジックを視覚的に理解しやすくなるため、読みやすくなります。

    ```javascript
    // bad
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // bad
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // bad
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // bad
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // good
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }

    // good
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1();
    }

    // good
    if (foo === 123 && bar === 'abc') {
      thing1();
    }
    ```

  <a name="control-statement--value-selection"></a><a name="control-statements--value-selection"></a>

  - [17.2](#control-statements--value-selection) 後続処理を選択するための演算子を制御文の代わりに使用しない。

    ```javascript
    // bad
    !isRunning && startRunning();

    // good
    if (!isRunning) {
      startRunning();
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Comments

  <a name="comments--multiline"></a><a name="17.1"></a>

  - [18.1](#comments--multiline) 複数行のコメントには`/ ** ... * /`を使用すること。

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```
  <a name="comments--singleline"></a><a name="17.2"></a>

  - [18.2](#comments--singleline) 単一行コメントには`//`を使用すること。コメントを加えたいコードの上部に配置すること。また、コメントの前に空行を入ること。

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }
    ```

  <a name="comments--spaces"></a>

  - [18.3](#comments--spaces) 読みやすくするために、すべてのコメントをスペースで始めること。eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // bad
    //is current tab
    const active = true;

    // good
    // is current tab
    const active = true;

    // bad
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--actionitems"></a><a name="17.3"></a>

  - [18.4](#comments--actionitems) 問題を指摘して再考を促す場合や、問題の解決策を提案する場合など、コメントの前に`FIXME`や`TODO`を付けることで他の開発者の素早い理解を助けることができます。これらは、何らかのアクションを伴うという意味で通常のコメントとは異なります。アクションとは`FIXME -- 解決策が必要`もしくは`TODO -- 実装が必要`です。

  <a name="comments--fixme"></a><a name="17.4"></a>
  - [18.5](#comments--fixme) Use `// FIXME:` to annotate problems.
  - [18.5](#comments--fixme) 問題に対する注釈として`// FIXME:`を使用すること。

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: shouldn't use a global here
        // FIXME: グローバル変数を使用するべきではない。
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="17.5"></a>

  - [18.6](#comments--todo) 問題の解決策に対する注釈として`// TODO:`を使用すること。

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total should be configurable by an options param
        // TODO: total はオプションパラメータとして設定されるべき。
        this.total = 0;
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Whitespace

  <a name="whitespace--spaces"></a><a name="18.1"></a>

  - [19.1](#whitespace--spaces) 2スペースに設定されたソフトタブ（スペース文字）を使用すること。eslint: [`indent`](https://eslint.org/docs/rules/indent.html)

    ```javascript
    // bad
    function() {
    ∙∙∙∙const name;
    }

    // bad
    function() {
    ∙const name;
    }

    // good
    function() {
    ∙∙const name;
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="18.2"></a>
 
  - [19.2](#whitespace--before-blocks) 重要な中括弧（`{}`）の前にはスペースを1つ入れること。eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html)

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="18.3"></a>
 
  - [19.3](#whitespace--around-keywords) 制御構文（`if`文や`while`文など）の丸括弧（`()`）の前にはスペースを1つ入れること。関数宣言や関数呼び出し時の引数リストの前にはスペースは入れない。eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="18.4"></a>

  - [19.4](#whitespace--infix-ops) 演算子の間はスペースを入れること。eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  <a name="whitespace--newline-at-end"></a><a name="18.5"></a>

  - [19.5](#whitespace--newline-at-end) ファイルの最後は改行文字を1つ入れること。eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

    ```javascript
    // bad
    import { es6 } from './FooStyleGuide';
      // ...
    export default es6;
    ```

    ```javascript
    // bad
    import { es6 } from './FooStyleGuide';
      // ...
    export default es6;↵
    ↵
    ```

    ```javascript
    // good
    import { es6 } from './FooStyleGuide';
      // ...
    export default es6;↵
    ```

  <a name="whitespace--chains"></a><a name="18.6"></a>

  - [19.6](#whitespace--chains) 長くメソッドを連結する場合はインデントを使用すること。行がメソッド呼び出しではなく、新しい文であることを強調するために先頭にドット(.)を配置すること。eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led').data(data);
    ```

  <a name="whitespace--after-blocks"></a><a name="18.7"></a>
  
  - [19.7](#whitespace--after-blocks) 文の前とブロックの後には改行を残すこと。

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // good
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // bad
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // good
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="18.8"></a>

  - [19.8](#whitespace--padded-blocks) ブロックに空行を挟み込まない。eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html)

    ```javascript
    // bad
    function bar() {

      console.log(foo);

    }

    // also bad
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // good
    function bar() {
      console.log(foo);
    }

    // good
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-parens"></a><a name="18.9"></a>
 
  - [19.9](#whitespace--in-parens) 丸括弧(`()`)の内側にスペースを追加しない。

    ```javascript
    // bad
    function bar( foo ) {
      return foo;
    }

    // good
    function bar(foo) {
      return foo;
    }

    // bad
    if ( foo ) {
      console.log(foo);
    }

    // good
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="18.10"></a>

  - [19.10](#whitespace--in-brackets) 角括弧(`[]`)の内側にスペースを追加しない。eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html)

    ```javascript
    // bad
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // good
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="18.11"></a>
  
  - [19.11](#whitespace--in-braces) 中括弧(`{}`)の内側にスペースを追加すること。eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html)

    ```javascript
    // bad
    const foo = {clark: 'kent'};

    // good
    const foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="18.12"></a>

  - [19.12](#whitespace--max-len) 100文字を超えるコード行（空白を含む）を避ける。注：[上記](#strings--line-length)のとおり、長い文字列はこの規則から除外されており、分割しない。eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

    > 原因： これは読みやすさと保守性を保証します。

    ```javascript
    // bad
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // bad
    $.ajax({ method: 'POST', url: 'https://www.google.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // good
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // good
    $.ajax({
      method: 'POST',
      url: 'https://www.google.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

  <a name="whitespace--block-spacing"></a>

  - [19.13](#whitespace--block-spacing) 開いている中括弧(`{`)と同じ行の次の中括弧(`}`)の間に一定のスペースを空けること。この規則は、同じ行の閉じ中括弧(`}`)と前の中括弧(`{`)との間に一貫性のあるスペースを適用します。eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

    ```javascript
    // bad
    function foo() {return true;}
    if (foo) { bar = 0;}

    // good
    function foo() { return true; }
    if (foo) { bar = 0; }
    ```

  <a name="whitespace--comma-spacing"></a>

  - [19.14](#whitespace--comma-spacing) コンマの前にスペースを入れず、コンマの後にスペースを入れること。eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

    ```javascript
    // bad
    var foo = 1,bar = 2;
    var arr = [1 , 2];

    // good
    var foo = 1, bar = 2;
    var arr = [1, 2];
    ```

  <a name="whitespace--computed-property-spacing"></a>

  - [19.15](#whitespace--computed-property-spacing) 計算プロパティの括弧の中にスペースは入れないこと。eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

    ```javascript
    // bad
    obj[foo ]
    obj[ 'foo']
    var x = {[ b ]: a}
    obj[foo[ bar ]]

    // good
    obj[foo]
    obj['foo']
    var x = { [b]: a }
    obj[foo[bar]]
    ```

  <a name="whitespace--func-call-spacing"></a>

  - [19.16](#whitespace--func-call-spacing) 関数とその呼び出しの間にスペースを入れない。eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

    ```javascript
    // bad
    func ();

    func
    ();

    // good
    func();
    ```

  <a name="whitespace--key-spacing"></a>

  - [19.17](#whitespace--key-spacing) オブジェクトリテラルプロパティのキーと値の間のスペースを入れること。eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

    ```javascript
    // bad
    var obj = { "foo" : 42 };
    var obj2 = { "foo":42 };

    // good
    var obj = { "foo": 42 };
    ```

  <a name="whitespace--no-trailing-spaces"></a>

  - [19.18](#whitespace--no-trailing-spaces) 行末に末尾のスペースを入れない。eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

  <a name="whitespace--no-multiple-empty-lines"></a>

  - [19.19](#whitespace--no-multiple-empty-lines) 複数の空行を避け、ファイルの終わりには改行を1つだけ入れること。eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // bad
    var x = 1;



    var y = 2;

    // good
    var x = 1;

    var y = 2;
    ```
    <!-- markdownlint-enable MD012 -->

**[⬆ back to top](#table-of-contents)**

## Commas

  <a name="commas--leading-trailing"></a><a name="19.1"></a>

  - [20.1](#commas--leading-trailing) 先頭のカンマは**禁止**。eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime,
    ];

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  <a name="commas--dangling"></a><a name="19.2"></a>

  - [20.2](#commas--dangling) 末尾のカンマ **いいね:)**。eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle.html)

    > 原因： これはよりきれいなgit diffにつながります。また、Babelのようなトランスパイラーは、トランスコードされたコード内の追加の末尾のコンマを削除します。つまり、古いブラウザの末尾のコンマの問題を心配する必要はありません。

    ```diff
    // bad - git diff without trailing comma
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    };

    // good - git diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };
    ```

    ```javascript
    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];

    // bad
    function createHero(
      firstName,
      lastName,
      inventorOf
    ) {
      // does nothing
    }

    // good
    function createHero(
      firstName,
      lastName,
      inventorOf,
    ) {
      // does nothing
    }

    // good (note that a comma must not appear after a "rest" element)
    function createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    ) {
      // does nothing
    }

    // bad
    createHero(
      firstName,
      lastName,
      inventorOf
    );

    // good
    createHero(
      firstName,
      lastName,
      inventorOf,
    );

    // good (note that a comma must not appear after a "rest" element)
    createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    );
    ```

**[⬆ back to top](#table-of-contents)**

## Semicolons

  <a name="semicolons--required"></a><a name="20.1"></a>

  - [21.1](#semicolons--required) **もちろん**。eslint: [`semi`](https://eslint.org/docs/rules/semi.html)

    > 原因： JavaScriptはセミコロンなしで改行を検出すると、[自動セミコロン挿入(Automatic Semicolon Insertion)](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion)と呼ばれる一連の規則を使用して、その改行をステートメントの終わりと見なし、（名前が示すとおり）改行の前にセミコロンを入れなければその行が壊れると考えた場所に、セミコロンを配置するかどうかを決定します。ただし、ASIにはいくつかの風変わりな動作が含まれており、JavaScriptが改行を誤って解釈した場合、コードは壊れます。新機能がJavaScriptの一部になるにつれて、これらのルールはより複雑になります。ステートメントを明示的に終了し、不足しているセミコロンを検知するようにリンターを構成すると、問題に遭遇するのを防ぐのに役立ちます。

    ```javascript
    // bad - raises exception
    const luke = {}
    const leia = {}
    [luke, leia].forEach(jedi => jedi.father = 'vader')

    // bad - raises exception
    const reaction = "No! That’s impossible!"
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // bad - returns `undefined` instead of the value on the next line - always happens when `return` is on a line by itself because of ASI!
    // 次の行の値の代わりに`undefined`を返します - ASIであるがため、`return`が単独で行にあるときは必ずこれは起こります！
    function foo() {
      return
        'search your feelings, you know it to be foo'
    }

    // good
    const luke = {};
    const leia = {};
    [luke, leia].forEach((jedi) => {
      jedi.father = 'vader';
    });

    // good
    const reaction = "No! That’s impossible!";
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }());

    // good
    function foo() {
      return 'search your feelings, you know it to be foo';
    }
    ```

    [Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ back to top](#table-of-contents)**

## Type Casting & Coercion

  <a name="coercion--explicit"></a><a name="21.1"></a>

  - [22.1](#coercion--explicit) 文の先頭で型の強制を行うこと。

  <a name="coercion--strings"></a><a name="21.2"></a>
  - [22.2](#coercion--strings) Strings: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)
  - [22.2](#coercion--strings) 文字列の場合: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    // => this.reviewScore = 9;

    // bad
    const totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

    // bad
    const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

    // bad
    const totalScore = this.reviewScore.toString(); // isn’t guaranteed to return a string

    // good
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="21.3"></a>

  - [22.3](#coercion--numbers) 数値の場合: 型変換には`Number`を使用すること。`parseInt`を使用する場合、常に型変換のための基数を引数に渡すこと。eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // bad
    const val = parseInt(inputValue);

    // good
    const val = Number(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    ```

  <a name="coercion--comment-deviations"></a><a name="21.4"></a>

  - 何らかの理由であなたがワイルドなことをしていて、`parseInt`がボトルネックであり、[パフォーマンス上の理由](https://jsperf.com/coercion-vs-casting/3)からBitshiftを使用する必要がある場合は、その理由(why)と対処方法(what)を説明するコメントを残してください。

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     * parseIntがボトルネックとなっていたため、
     * ビットシフトで文字列を数値へ強制的に変換することで
     * パフォーマンスを改善させます。
     */
    const val = inputValue >> 0;
    ```

  <a name="coercion--bitwise"></a><a name="21.5"></a>
  - [22.5](#coercion--bitwise) **注意** ビットシフト操作を使用するときは注意してください。数値は[64ビット値]((https://es5.github.io/#x4.3.19))として表されますが、ビットシフト操作は常に32ビット整数を返します。[ここから](https://es5.github.io/#x11.7)。ビットシフトは、32ビットを超える整数値に対して予期しない動作を引き起こす可能性があります。符号付き32ビット整数の最大値は2,147,483,647です。

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  <a name="coercion--booleans"></a><a name="21.6"></a>

  - [22.6](#coercion--booleans) 真偽値の場合: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // good
    const hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions

  <a name="naming--descriptive"></a><a name="22.1"></a>

  - [23.1](#naming--descriptive) 1文字の名前は避ける。名前から意図が読み取れるようにすること。eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

    ```javascript
    // bad
    function q() {
      // ...
    }

    // good
    function query() {
      // ...
    }
    ```

  <a name="naming--camelCase"></a><a name="22.2"></a>

  - [23.2](#naming--camelCase) オブジェクト、関数、インスタンスにはキャメルケース（小文字から始まる）を使用すること。eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html)

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="22.3"></a>
 
  - [23.3](#naming--PascalCase) クラスやコンストラクタにはパスカルケース（大文字から始まる）を使用すること。eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  <a name="naming--leading-underscore"></a><a name="22.4"></a>

  - [23.4](#naming--leading-underscore) 末尾または先頭のアンダースコアを避ける。eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

    > 原因： JavaScriptは、プロパティやメソッドについてのプライバシーの概念を持っていません。先頭のアンダースコアは「非公開」を意味する一般的な規則ですが、実際にはこれらのプロパティは完全に公開されているため公開APIの一部です。この規約により、開発者は変更が壊れているとは見なされない、またはテストが必要ではないと誤って考える可能性があります。tl;dr: あなたが何かを「非公開」にしたいのであれば、それは明らかに存在してはいけません。

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // good
    this.firstName = 'Panda';

    // good, in environments where WeakMaps are available
    // see https://kangax.github.io/compat-table/es6/#test-WeakMap
    const firstNames = new WeakMap();
    firstNames.set(this, 'Panda');
    ```

  <a name="naming--self-this"></a><a name="22.5"></a>

  - [23.5](#naming--self-this) `this`の参照を保存するのを避ける。アロー関数か[Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)を利用すること。

    ```javascript
    // bad
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // bad
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  <a name="naming--filename-matches-export"></a><a name="22.6"></a>
 
  - [23.6](#naming--filename-matches-export) ファイルを1つのクラスとしてexportする場合、ファイル名はクラス名と完全に一致させること。

    ```javascript
    // file 1 contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // file 2 contents
    export default function fortyTwo() { return 42; }

    // file 3 contents
    export default function insideDirectory() {}

    // in some other file
    // bad
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // bad
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // good
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    // ^ supports both insideDirectory.js and insideDirectory/index.js
    ```

  <a name="naming--camelCase-default-export"></a><a name="22.7"></a>

  - [23.7](#naming--camelCase-default-export) Default exportが関数の場合、キャメルケース（小文字から始まる）を使用すること。ファイル名は関数名と同じにすること。

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  <a name="naming--PascalCase-singleton"></a><a name="22.8"></a>

  - [23.8](#naming--PascalCase-singleton) シングルトン / function library / 単なるオブジェクトをexportする場合、パスカルケース（大文字から始まる）を使用すること。

    ```javascript
    const FooStyleGuide = {
      es6: {
      }
    };

    export default FooStyleGuide;
    ```

  <a name="naming--Acronyms-and-Initialisms"></a>

  - [23.9](#naming--Acronyms-and-Initialisms) 略語およびinitialisms（複数の単語で構成される言葉の頭文字をとって一つの単語にしたもの）は、常にすべて大文字、またはすべて小文字にする必要があります。

    > 原因： 名前は読みやすくするためのもので、コンピュータのアルゴリズムをなだめるものではありません。

    ```javascript
    // bad
    import SmsContainer from './containers/SmsContainer';

    // bad
    const HttpRequests = [
      // ...
    ];

    // good
    import SMSContainer from './containers/SMSContainer';

    // good
    const HTTPRequests = [
      // ...
    ];

    // also good
    const httpRequests = [
      // ...
    ];

    // best
    import TextMessageContainer from './containers/TextMessageContainer';

    // best
    const requests = [
      // ...
    ];
    ```

  <a name="naming--uppercase"></a>

  - [23.10](#naming--uppercase) （1）エクスポートされ、（2）`const`（再割り当てできない）であり、そして（3）プログラマがそれを（そしてネストされたプロパティも）変更しないと信頼できる場合に限り、定数を大文字にすることができます。

    > 原因： これは、変数が変更される可能性があるかどうかをプログラマが確信が持てないような状況で役立つ追加ツールです。UPPERCASE_VARIABLESは、変数（およびそのプロパティ）が変更されないように信頼できることをプログラマに知らせています。
    - すべての`const`変数はどうですか？ - これは不要です。ファイル内の定数には大文字を使用しないでください。ただしエクスポートされた定数には使用する必要があります。
    - エクスポートされたオブジェクトはどうですか？ - エクスポートの最上位レベルでは大文字（例えば `EXPORTED_OBJECT.key`）で、ネストされたすべてのプロパティが変更されないようにします。

    ```javascript
    // bad
    const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

    // bad
    export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

    // bad
    export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

    // ---

    // allowed but does not supply semantic value
    export const apiKey = 'SOMEKEY';

    // better in most cases
    export const API_KEY = 'SOMEKEY';

    // ---

    // bad - unnecessarily uppercases key while adding no semantic value
    export const MAPPING = {
      KEY: 'value'
    };

    // good
    export const MAPPING = {
      key: 'value'
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Accessors

  <a name="accessors--not-required"></a><a name="23.1"></a>

  - [24.1](#accessors--not-required) プロパティのためのアクセサ（Accessor）関数は必須ではありません。

  <a name="accessors--no-getters-setters"></a><a name="23.2"></a>

  - [24.2](#accessors--no-getters-setters) 予期しない副作用を引き起こし、テスト、保守、および理由の説明が難しいため、JavaScriptのゲッター/セッターは使用しないこと。代わりに、アクセサ関数を作るのであれば、`getVal()`とか `setVal('hello')`とすること、

    ```javascript
    // bad
    class Dragon {
      get age() {
        // ...
      }

      set age(value) {
        // ...
      }
    }

    // good
    class Dragon {
      getAge() {
        // ...
      }

      setAge(value) {
        // ...
      }
    }
    ```

  <a name="accessors--boolean-prefix"></a><a name="23.3"></a>

  - [24.3](#accessors--boolean-prefix) プロパティ/メソッドが`boolean`の場合は、`isVal()`または`hasVal()`を使用すること。

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  <a name="accessors--consistent"></a><a name="23.4"></a>
 
  - [24.4](#accessors--consistent) 一貫していれば、`get()`や`set()`という関数を作成することを認める。

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Events

  <a name="events--hash"></a><a name="24.1"></a>

  - [25.1](#events--hash) データペイロードをイベント（DOMイベントまたはBackboneイベントのようなもっと独自のものかどうかにかかわらず）に添付するときは、生の値の代わりにオブジェクトリテラル（「ハッシュ」とも呼ばれる）を渡すこと。これにより、後の開発者は、イベントのすべてのハンドラを見つけて更新することなく、イベントペイロードにさらにデータを追加できます。たとえば、下の例より

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:
    こちらの方が好まれます:

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingId: listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ back to top](#table-of-contents)**

## jQuery

  <a name="jquery--dollar-prefix"></a><a name="25.1"></a>

  - [26.1](#jquery--dollar-prefix) jQueryオブジェクトの変数は、先頭に`$`を付与すること。

    ```javascript
    // bad
    const sidebar = $('.sidebar');

    // good
    const $sidebar = $('.sidebar');

    // good
    const $sidebarBtn = $('.sidebar-btn');
    ```

  <a name="jquery--cache"></a><a name="25.2"></a>
 
  - [26.2](#jquery--cache) jQueryの検索結果をキャッシュすること。

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  <a name="jquery--queries"></a><a name="25.3"></a>

  - [26.3](#jquery--queries) DOMの検索には、`$('.sidebar ul')`や`$('.sidebar > ul')`のカスケードを使用すること。[jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)

  <a name="jquery--find"></a><a name="25.4"></a>

  - [26.4](#jquery--find) jQueryオブジェクトの検索には、スコープ付きの`find`を使用すること。

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#table-of-contents)**

## ECMAScript 5 Compatibility

  <a name="es5-compat--kangax"></a><a name="26.1"></a>
 
  - [27.1](#es5-compat--kangax) [Kangax](https://twitter.com/kangax/)の ES5[互換性表](http://kangax.github.io/es5-compat-table/)を参照すること。

**[⬆ back to top](#table-of-contents)**

<a name="ecmascript-6-styles"></a>
## ECMAScript 6+ (ES 2015+) Styles

  <a name="es6-styles"></a><a name="27.1"></a>

  - [28.1](#es6-styles) これは、さまざまなES6+機能へのリンク集です。

1. [Arrow Functions](#arrow-functions)
1. [Classes](#classes--constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Exponentiation Operator](#es2016-properties--exponentiation-operator)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

  <a name="tc39-proposals"></a>
 
  - [28.2](#tc39-proposals) ステージ3に達していない[TC39提案](https://github.com/tc39/proposals)を使用しない。

    > 原因： [それらは最終決定されていません](https://tc39.github.io/process-document/)。そしてそれらは変更されるか、または完全に撤回される可能性があります。JavaScriptを使用したいのですが、提案はまだJavaScriptではありません。

**[⬆ back to top](#table-of-contents)**

## Standard Library

  [標準ライブラリ](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects)には機能的には壊れているがレガシーな理由で残っているユーティリティが含まれています。

  <a name="standard-library--isnan"></a>

  - [29.1](#standard-library--isnan) グローバルな`isNaN`の代わりに`Number.isNaN`を使用すること。
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > 原因： グローバルな`isNaN`は数字ではないものを数字に強制変換し、NaNに強制変換されたものすべてにtrueを返します。
    > この動作が望ましい場合は、それを明示にしてください。

    ```javascript
    // bad
    isNaN('1.2'); // false
    isNaN('1.2.3'); // true

    // good
    Number.isNaN('1.2.3'); // false
    Number.isNaN(Number('1.2.3')); // true
    ```

  <a name="standard-library--isfinite"></a>

  - [29.2](#standard-library--isfinite) グローバルな`isFinite`の代わりに`Number.isFinite`を使用する。
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > 原因： グローバルな`isFinite`は数字ではないものを数字に強制変換し、有限数に強制変換されたものすべてにtrueを返します。
    > この動作が望ましい場合は、それを明示にしてください。

    ```javascript
    // bad
    isFinite('2e3'); // true

    // good
    Number.isFinite('2e3'); // false
    Number.isFinite(parseInt('2e3', 10)); // true
    ```

**[⬆ back to top](#table-of-contents)**