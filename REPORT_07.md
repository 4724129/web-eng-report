# 第7回 Webエンジニアリング演習 レポート

## 学籍番号
4724129

## 2画面の見出しと本文

`csr/index.html` に「比較」「詳細」のリンクを置き、`location.hash` の値によって `#app` の中身を切り替えた。

| URL | 見出し | 本文 |
| --- | --- | --- |
| `/csr/#compare` | 課題用画面 比較 | この文章は比較画面です。 |
| `/csr/#detail` | 課題用画面 詳細 | この文章は詳細画面です。 |
| `/csr/`（ハッシュなし） | 課題用画面 | 上のリンクから画面を選んでください。 |

`csr/index.html` のリンク部分:

```html
<nav>
    <a href="#compare">比較</a>
    <a href="#detail">詳細</a>
</nav>
<main id="app"></main>
```

`csr/app.ts` の描画部分:

```ts
const render = (): void => {
    const page = location.hash;

    if (page === "#detail") {
        app.innerHTML = `
            <h1>課題用画面 詳細</h1>
            <p>この文章は詳細画面です。</p>
        `;
    } else if (page === "#compare") {
        app.innerHTML = `
            <h1>課題用画面 比較</h1>
            <p>この文章は比較画面です。</p>
        `;
    } else {
        app.innerHTML = `
            <h1>課題用画面</h1>
            <p>上のリンクから画面を選んでください。</p>
        `;
    }
};
```

## クリック後のURLとNetworkの確認

- 「比較」をクリックすると、アドレスバーの末尾が `#compare` に変わった。
- 「詳細」をクリックすると、アドレスバーの末尾が `#detail` に変わった。
- ChromeのDevToolsでNetworkタブを開いた状態でリンクをクリックしたが、新しいHTMLのリクエストは増えなかった。
- 最初に読み込んだ `index.html` と `app.js` だけで、画面の切り替えがブラウザ内で完結していることが分かった。

`#` 以降は同じページの中の位置を表すため、値が変わってもサーバへ新しいHTMLを要求しない。そのため、画面の切り替えにサーバとの通信は発生しなかった。

## 画面を切り替える処理の要点

1. `location.hash` を読む
   - リンク `href="#detail"` を押すと、`location.hash` が `#detail` になる。この値で、どの画面を表示するかを決める。
2. `innerHTML` で `#app` の中身を書き換える
   - `if / else if / else` で `location.hash` の値ごとに分岐し、`app.innerHTML` に見出しと本文を代入する。
3. `hashchange` イベントで関数を実行する
   - `window.addEventListener("hashchange", render)` により、ハッシュが変わるたびに `render` が呼ばれる。
4. 最初の表示でも `render()` を1回呼ぶ
   - `hashchange` はハッシュが変わったときにしか起きない。そのため、ページを開いた直後や、`#detail` 付きのURLを直接開いたときにも正しい画面が出るよう、最後に `render()` を直接呼んでいる。
5. `null` チェックをする
   - `document.querySelector("#app")` は見つからない場合に `null` を返す。TypeScriptの型検査を通すため、先に `null` を除いている。

この方式は、最初のHTMLに見出しも本文も含まれず、JavaScriptの実行後に画面が組み立てられるため、CSR（クライアントサイドレンダリング）の例である。
