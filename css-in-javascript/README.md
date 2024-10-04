# CSS-in-JavaScript Style Guide

*A mostly reasonable approach to CSS-in-JavaScript*

## Table of Contents

1. [Naming](#naming)
1. [Ordering](#ordering)
1. [Nesting](#nesting)
1. [Inline](#inline)
1. [Themes](#themes)

## Naming

- オブジェクトのキーにはキャメルケースを利用すること。(i.e. "selectors").

> 原因：このキーをコンポーネントの中で`styles`オブジェクトのプロパティとして利用します。これにはキャメルケースを使うことが最も便利です。

```js
// bad
{
  'bermuda-triangle': {
    display: 'none',
  },
}

// good
{
  bermudaTriangle: {
    display: 'none',
  },
}
```

- アンダースコアを異なるスタイルのモディファイアとして利用すること。

> 原因：BEMと同様です。この命名の慣例は、アンダースコアの前にある要素のスタイルを変更することを明確にします。また、アンダースコアは引用符を必要としないため、他の文字よりも好まれます。

```js
// bad
{
  bruceBanner: {
    color: 'pink',
    transition: 'color 10s',
  },

  bruceBannerTheHulk: {
    color: 'green',
  },
}

// good
{
  bruceBanner: {
    color: 'pink',
    transition: 'color 10s',
  },

  bruceBanner_theHulk: {
    color: 'green',
  },
}
```

- `selectorName_fallback`の形式をフォールバックスタイルとして利用すること。

> 原因：モディファイアと同様です。名前の一貫性を維持することは、どのスタイルがブラウザの適当なスタイルで上書きされるかの関連性を明らかにするのに役立ちます。

```js
// bad
{
  muscles: {
    display: 'flex',
  },

  muscles_sadBears: {
    width: '100%',
  },
}

// good
{
  muscles: {
    display: 'flex',
  },

  muscles_fallback: {
    width: '100%',
  },
}
```

- フォールバックスタイルを設定するために個々のセレクタを利用すること。

> 原因：別々のオブジェクトにフォールバックスタイルを定義すると、目的が明確になり可読性があがります。

```js
// bad
{
  muscles: {
    display: 'flex',
  },

  left: {
    flexGrow: 1,
    display: 'inline-block',
  },

  right: {
    display: 'inline-block',
  },
}

// good
{
  muscles: {
    display: 'flex',
  },

  left: {
    flexGrow: 1,
  },

  left_fallback: {
    display: 'inline-block',
  },

  right_fallback: {
    display: 'inline-block',
  },
}
```

- デバイスに依存しない名前(e.g. "small", "medium", and "large")をメディアクエリのブレイクポイント名に利用すること。

> 原因：一般的に利用される"phone", "tablet", "desktop"などの名前は現実世界のデバイスの特性と一致していません。これらの名前を使うことで間違った期待を抱かせることになります。

```js
// bad
const breakpoints = {
  mobile: '@media (max-width: 639px)',
  tablet: '@media (max-width: 1047px)',
  desktop: '@media (min-width: 1048px)',
};

// good
const breakpoints = {
  small: '@media (max-width: 639px)',
  medium: '@media (max-width: 1047px)',
  large: '@media (min-width: 1048px)',
};
```

## Ordering

- コンポーネントの後にスタイルを定義すること。

> 原因：私たちは、当たり前のようにコンポーネント定義の後で利用されるhigher-orderコンポーネントを使ってスタイルを装飾します。スタイルオブジェクトを直接この関数に渡すことで、間接的な参照(訳注:不要な中間変数のことだと思われる)を減らすことができます。

```jsx
// bad
const styles = {
  container: {
    display: 'inline-block',
  },
};

function MyComponent({ styles }) {
  return (
    <div {...css(styles.container)}>
      Never doubt that a small group of thoughtful, committed citizens can
      change the world. Indeed, it’s the only thing that ever has.
    </div>
  );
}

export default withStyles(() => styles)(MyComponent);

// good
function MyComponent({ styles }) {
  return (
    <div {...css(styles.container)}>
      Never doubt that a small group of thoughtful, committed citizens can
      change the world. Indeed, it’s the only thing that ever has.
    </div>
  );
}

export default withStyles(() => ({
  container: {
    display: 'inline-block',
  },
}))(MyComponent);
```

## Nesting

- 同じインデントのレベルの中の隣接するブロックとの間に空行を残すこと。

> 原因：この空白が可読性を向上させ、マージの競合の可能性を減らすことができます。

```js
// bad
{
  bigBang: {
    display: 'inline-block',
    '::before': {
      content: "''",
    },
  },
  universe: {
    border: 'none',
  },
}

// good
{
  bigBang: {
    display: 'inline-block',

    '::before': {
      content: "''",
    },
  },

  universe: {
    border: 'none',
  },
}
```

## Inline

- ユニーク性が高い(propsの値を利用するなど)スタイルにはインラインスタイルを利用して、そうではない場合には利用しないこと。

> 原因：テーマ用のスタイルシートを作成することは作成コストが高くなるため、ユニーク性の高いスタイルのセットにこそ利用するのが最適です。

```jsx
// bad
export default function MyComponent({ spacing }) {
  return (
    <div style={{ display: 'table', margin: spacing }} />
  );
}

// good
function MyComponent({ styles, spacing }) {
  return (
    <div {...css(styles.periodic, { margin: spacing })} />
  );
}
export default withStyles(() => ({
  periodic: {
    display: 'table',
  },
}))(MyComponent);
```

## Themes

- *react-with-stylesは`withStyles()`, `ThemedStyleSheet`, や `css()`といった機能を提供します。これらはこのドキュメントのいくるかの例の中で利用されています。*

> 原因：これはスタイリングされたコンポーネントで変数を共有するために便利です。抽象的なレイヤーを利用することでより利便性があがります。これにより、コンポーネントがいかなる基礎的な実装とも密接に結合することを防ぎ、より自由度が増します。

- 色はテーマの中でのみ定義すること。

```js
// bad
export default withStyles(() => ({
  chuckNorris: {
    color: '#bada55',
  },
}))(MyComponent);

// good
export default withStyles(({ color }) => ({
  chuckNorris: {
    color: color.badass,
  },
}))(MyComponent);
```

- フォントはテーマの中でのみ定義すること。

```js
// bad
export default withStyles(() => ({
  towerOfPisa: {
    fontStyle: 'italic',
  },
}))(MyComponent);

// good
export default withStyles(({ font }) => ({
  towerOfPisa: {
    fontStyle: font.italic,
  },
}))(MyComponent);
```

- 関連するスタイルのセットとしてフォントと関連するスタイルを定義すること。

```js
// bad
export default withStyles(() => ({
  towerOfPisa: {
    fontFamily: 'Italiana, "Times New Roman", serif',
    fontSize: '2em',
    fontStyle: 'italic',
    lineHeight: 1.5,
  },
}))(MyComponent);

// good
export default withStyles(({ font }) => ({
  towerOfPisa: {
    ...font.italian,
  },
}))(MyComponent);
```

- 基準となるグリッドの単位(値か乗算関数を引数にとるもののどちらでも良い)をテーマ内に定義すること。

```js
// bad
export default withStyles(() => ({
  rip: {
    bottom: '-6912px', // 6 feet
  },
}))(MyComponent);

// good
export default withStyles(({ units }) => ({
  rip: {
    bottom: units(864), // 6 feet, assuming our unit is 8px
  },
}))(MyComponent);

// good
export default withStyles(({ unit }) => ({
  rip: {
    bottom: 864 * unit, // 6 feet, assuming our unit is 8px
  },
}))(MyComponent);
```

- メディアクエリはテーマの中でのみ定義すること。

```js
// bad
export default withStyles(() => ({
  container: {
    width: '100%',

    '@media (max-width: 1047px)': {
      width: '50%',
    },
  },
}))(MyComponent);

// good
export default withStyles(({ breakpoint }) => ({
  container: {
    width: '100%',

    [breakpoint.medium]: {
      width: '50%',
    },
  },
}))(MyComponent);
```

- 特殊はフォールバックはテーマの中に定義すること。

> 原因：多くのCSS-in-JavaScript実装者たちは、同じプロパティ(e.g. `display`)を特別にフォールバックさせるために、スタイルオブジェクトを少しトリッキーにマージします。このアプローチを統一するために、これらのフォールバックをテーマに入れます。

```js
// bad
export default withStyles(() => ({
  .muscles {
    display: 'flex',
  },

  .muscles_fallback {
    'display ': 'table',
  },
}))(MyComponent);

// good
export default withStyles(({ fallbacks }) => ({
  .muscles {
    display: 'flex',
  },

  .muscles_fallback {
    [fallbacks.display]: 'table',
  },
}))(MyComponent);

// good
export default withStyles(({ fallback }) => ({
  .muscles {
    display: 'flex',
  },

  .muscles_fallback {
    [fallback('display')]: 'table',
  },
}))(MyComponent);
```

- カスタムテーマの作成は可能な限り少なくすること。主要なアプリケーションはただ１つのテーマのみとすること。

- ユニークでわかりやすいキーを利用して、ネストされたオブジェクトの下を、カスタムテーマ設定の名前空間とすること。

```js
// bad
ThemedStyleSheet.registerTheme('mySection', {
  mySectionPrimaryColor: 'green',
});

// good
ThemedStyleSheet.registerTheme('mySection', {
  mySection: {
    primaryColor: 'green',
  },
});
```

---