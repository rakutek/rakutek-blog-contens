---
title: ""
emoji: "🐈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

最近、Tailwind 流行ってますよね
個人的な意見として、Tailwind を初めて触った時に、

- 公式サイト: https://chakra-ui.com/
- GitHub: https://github.com/chakra-ui/chakra-ui

Chakra UI は emotion がベース

## 準備

```shell
yarn create next-app --example=with-typescript chakraui-test

cd chakraui-test

yarn add @chakra-ui/react @emotion/react @emotion/styled framer-motion
```

また、Chakra UI を使うコンポーネントは`<ChakraProvider>`で囲む必要があるため、\_app.tsx でルートのコンポーネントを囲います。

```js:_app.tsx
import type { AppProps } from "next/app";
import { ChakraProvider } from "@chakra-ui/react";

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <ChakraProvider>
      <Component {...pageProps} />
    </ChakraProvider>
  );
}

export default MyApp;
```

これだけです！css in js の emotion をベースにしているので、Tailwind に比べたら導入がラクですよね
Tailwind でも Chakra でもサードパーティー製のライブラリをコマンド一つで導入できる Blitz.js はいいぞ

Style Overrides 💫#
In most applications, it is a common challenge to want to override styles for a specific context to match design requirements.

Tailwind CSS: Given that Tailwind is a CSS utility framework, you may need to figure out the best way to override specific classNames, or write custom CSS.

Chakra UI: Given that Chakra UI styles are prop-based, overrides is as easy as passing a prop.

スタイルオーバーライド 💫#
ほとんどのアプリケーションでは、デザイン要件に合わせて特定のコンテキストのスタイルをオーバーライドしたいというのが一般的な課題となっています。

Tailwind CSS。Tailwind が CSS ユーティリティ フレームワークであることを考えると、特定の classNames をオーバーライドするための最良の方法を見つけ出すか、カスタム CSS を書く必要があるかもしれません。

Chakra UI。Chakra UI スタイルが prop ベースであることを考えると、オーバーライドは prop を渡すのと同じくらい簡単です。

ダークモード 🌜 ＃
Tailwind CSS：実験的

Chakra UI：すべてのコンポーネントはライトモードとダークモードに対応しています。また、アプリケーション全体で独自のライトモードとダークモードのエクスペリエンスを作成することもできます。

Design Principles
Chakra UI is established on principles that keep its components fairly consistent. Understanding these concepts will help you better contribute to Chakra UI.

Our goal is to design simple, composable components that cater to real-life UI design problems. In order to do that, we developed a set of principles that help us always be on that path.

Style Props: All component styles can be overriden or extended via style props to reduce the use of css prop or styled(). Compose new components from Box.

Simplicity: Strive to keep the component API fairly simple and show real world scenarios of using the component.

Composition: Break down components into smaller parts with minimal props to keep complexity low, and compose them together. This will ensure that the styles and functionality are flexible and extensible.

Accessibility: When creating a component, keep accessibility top of mind. This includes keyboard navigation, focus management, color contrast, voice over, and the correct aria-\* attributes.

Dark Mode: Make components dark mode compatible. Use our ColorModeProvider component and useColorMode hook to handle styling. Learn more about dark mode.

Naming Props: We all know naming is the hardest thing in this industry. Generally, ensure a prop name is indicative of what it does. Boolean props should be named using auxiliary verbs such as does, has, is and should. For example, Button uses isDisabled, isLoading, etc.

設計原理
Chakra UI は、そのコンポーネントを公平に一貫性のあるものにするための原則に基づいて確立されています。これらのコンセプトを理解することで、Chakra UI への貢献度を高めることができます。

私たちの目標は、現実の UI デザインの問題に対応したシンプルで構成可能なコンポーネントを設計することです。そのために、私たちが常にその道を歩むのに役立つ一連の原則を開発しました。

スタイルの小道具。すべてのコンポーネントのスタイルは、スタイル プロップを介してオーバーライドまたは拡張することで、CSS プロップや styled() の使用を減らすことができます。Box から新しいコンポーネントを構成します。

簡素化。コンポーネント API をかなりシンプルに保ち、コンポーネントを使用する実世界のシナリオを示すように努めています。

構成。複雑さを抑えるために、コンポーネントを最小限のプロップで小さなパーツに分解し、それらを一緒に構成します。これにより、スタイルと機能が柔軟で拡張性のあるものになります。

アクセシビリティ。コンポーネントを作成する際には、アクセシビリティを最優先に考えてください。これには、キーボードナビゲーション、フォーカス管理、カラーコントラスト、ボイスオーバー、正しい aria-\* 属性が含まれます。

ダークモード。コンポーネントをダークモードに対応させます。ColorModeProvider コンポーネントと useColorMode フックを使用してスタイリングを処理します。ダークモードの詳細については、こちらをご覧ください。

小道具の名前付け。この業界ではネーミングが最も難しいことは誰もが知っています。一般的に、小道具の名前は、それが何をするかを示すものであることを確認してください。ブール値の小道具は、does、has、is、should などの助動詞を使って命名します。例えば、Button は isDisabled、isLoading などを使います。
