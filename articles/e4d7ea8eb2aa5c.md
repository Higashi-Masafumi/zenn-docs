---
title: "Next.js turbopackでmdxを使う方法について"
emoji: "👌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nextjs", "turbopack", "mdx"]
published: true
---

# TurbopackでMDXを使う方法について
Next.jsでは、最近になってコンパイラーを[webpack](https://webpack.js.org/)から、[turbopack](https://nextjs.org/docs/app/api-reference/turbopack)に移行しつつある。
[webpack](https://webpack.js.org/)が`Javascript`で書かれているのに対して、[turbopack](https://nextjs.org/docs/app/api-reference/turbopack)は`Rust`で書かれており、ビルドが高速化されることが期待されている。

基本的に公式ドキュメントなどではまだ[webpack](https://webpack.js.org/)を利用する場合の内容が書かれていることが多い気がしている。
今回は`MDX`を`Next.js`で表示するためのドキュメントが`webpack`用で書かれていたので、それの`turbopack`用の内容について考えてみようと思う。

# Next.jsでMDXを使う方法についてのドキュメント

https://nextjs.org/docs/app/guides/mdx

詳細はこのドキュメントの内容を読めばある程度わかるので、省略します。
重要なのは`next.config.ts`の設定です。

以下のように設定します。

```ts:next.config.ts
import type { NextConfig } from 'next';
import createMDX from '@next/mdx'

const nextConfig: NextConfig = {
  experimental: {
    mdxRs: true,
  },
};

const withMDX = createMDX({
  // Add markdown plugins here, as desired
})

export default withMDX(nextConfig)
```

`experimental`のoptionで`mdxRs: true`を設定することが重要です。
これを設定しないとMDXファイルを読み込もうとしても`unknown module error`がでます。

# なんでこれで解決するのか？

`mdxRs: true`はmdxファイルを`Rust`でコンパイルするオプションです。`turbopack`も`Rust`でコンパイルするので、そこを合わせることで、コンパイルエラーを防ぐことができるのではないかと思います。

# どうでもいいこと
`turbopack`は`Rust`製で高速と言われているが、`Next.js`の開発環境で使っていてもそこまで高速とは感じません。
それよりもむしろ[Remix](https://remix.run/)や[Vite](https://ja.vite.dev/)で使われている`Go`製の[esbuild](https://esbuild.github.io/)の方が早いように感じてしまいます。使い方が悪いかもしれませんが、今後の改善に期待です。

https://esbuild.github.io/