# 第9回 Webエンジニアリング演習 レポート

## 学籍番号
（4724129）

## Stateの一覧と役割

`App` コンポーネントに、次の2つのStateを持たせた。

| State | 型 | 初期値 | 保持するもの |
|---|---|---|---|
| `title` | `string` | `""` | 入力欄に入力中の文字列 |
| `todos` | `Todo[]` | `initialTodos` | 画面に表示するTODOの一覧 |

- `title` は入力のたびに `setTitle` で更新され、`<input>` の `value` と「入力中」の表示に使われる。
- `todos` は追加時に `setTodos` で新しい配列へ更新され、`TodoList` へPropsとして渡される。
- TODOの件数は `todos` から計算できるため、別のStateにはしていない。

## 完了ボタンから画面更新まで

今回は、完了ボタン（手順5〜8）は実装していない。そのため、実装した「TODOの追加」について、操作から画面更新までの流れを説明する。

1. 利用者が「追加」ボタンを押す、またはEnterキーを押す。
2. `form` の `onSubmit` に登録した `handleSubmit` が呼ばれる。
3. `handleSubmit` が `setTodos` を呼び、現在の配列に新しいTODOを加えた次の配列を渡す。あわせて `setTitle("")` で入力欄を空にする。
4. Reactが新しいStateで `App` を再実行（再レンダリング）する。
5. 新しい `todos` が `TodoList` にPropsとして渡り、一覧に新しいTODOが表示される。

Stateの持ち主は `App` である。`TodoList` はPropsで受け取った配列を表示するだけで、直接変更はしない。値を覚えるState（`App`）と、値を渡すProps（`TodoList` へ）を組み合わせて画面が作られている。

## 空白入力を防ぐコードと確認結果

### コード

```tsx
function handleSubmit(event: FormEvent<HTMLFormElement>) {
  event.preventDefault();

  const normalizedTitle: string = title.trim();
  if (normalizedTitle === "") {
    return;
  }

  const newTodo: Todo = {
    id: Date.now(),
    title: normalizedTitle,
    completed: false,
  };
  setTodos((current) => [...current, newTodo]);
  setTitle("");
}
```

- `trim()` で前後の空白を取り除き、結果が空文字なら `return` で処理を終える。
- 判定を `newTodo` の作成と `setTodos` より前に置くことで、空のTODOが追加される前に処理を止めている。
- `newTodo` の `title` には、空白を取り除いた `normalizedTitle` を使う。

### 確認結果

| 入力・操作 | 期待する結果 | 実際の結果 |
|---|---|---|
| 空文字のまま送信 | 件数が変わらない | 追加されなかった（0件のまま） |
| 半角スペース3文字を送信 | 件数が変わらない | 追加されなかった（0件のまま） |
| 「 aaaaa 」を送信 | 前後の空白を除いて1件追加 | 前後の空白が除かれ、「aaaaa」だけが1件追加された |
| 「aaa aaa」をEnterで送信 | 1件追加し、入力欄が空になる |　一件増え、入力欄が空になった |

pnpm run buildは成功した。Consoleにエラーもなかった。
