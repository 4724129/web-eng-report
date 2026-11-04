# 第6回 Webエンジニアリング演習 レポート

## 学籍番号

4724129

## `Todo`型の定義

`src/types.ts`

```ts
export type Todo = {
  id: number;
  title: string;
  completed: boolean;
  dueDate?: string;
};
```

## 未完了件数を返す関数の引数と戻り値の型

`src/todo-service.ts`

```ts
export async function countIncompleteTodos(): Promise<number> {
  const currentTodos = await fetchTodos();
  const incompleteTodos = currentTodos.filter(
    (todo) => !todo.completed,
  );
  return incompleteTodos.length;
}
```

- 引数: なし
- 戻り値の型: `Promise<number>`

## 未完了件数の実行結果

`pnpm run start` の出力(最後の行が未完了件数)

[
{ id: 1, title: '型を定義する', completed: true },
{ id: 2, title: '関数を作る', completed: false }
]
関数を作る
[
{ id: 1, title: '型を定義する', completed: true },
{ id: 2, title: '関数を作る', completed: true }
]
未完了のTODO: 1件


元のデータではID 2だけが未完了のため、件数は1件。
