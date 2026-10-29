# 第5回 Webエンジニアリング演習 レポート

## 学籍番号
4724129

## TODO配列のコード
```ts
const todos = [
  { id: 1, title: "型を復習する", completed: true },
  { id: 2, title: "配列を学ぶ", completed: false },
  { id: 3, title: "レポートを書く", completed: false },
  { id: 4, title: "課題をpushする", completed: false },
];
```

## TODOを更新した条件
`for...of` で全TODOを1件ずつ取り出し、`if (!todo.completed)` で未完了のTODOだけを選んで `todo.completed = true` に更新した。

```ts
for (const todo of todos) {
  if (!todo.completed) {
    todo.completed = true;
    console.log(`更新後: ${todo.title} [完了]`);
  }
}
```

## 完了件数の更新前後の実行結果
```
更新後: 配列を学ぶ [完了]
更新後: レポートを書く [完了]
更新後: 課題をpushする [完了]
完了件数: 更新前 1件 -> 更新後 4件
```
