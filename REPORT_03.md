# 第3回 Webエンジニアリング演習　レポート
## 学籍番号
4724129
## イメージとコンテナの違い
Dockerイメージは、アプリのファイル・実行プログラム・設定をまとめた読み取り専用のパッケージであり、コンテナを作るための「型」に当たる。一度作ったイメージは変更せず、変更したいときは新しくビルドし直す。一方Dockerコンテナは、そのイメージから作られて実際に動いている実体。コンテナごとに名前、ネットワーク、書き込み領域を持ち、起動、停止、再開、削除ができる。この関係から同じイメージからweb-1, web-2のように名前の違うコンテナを複数起動することができる。またコンテナをdocker rmで削除しても元のイメージは残るので、何度でも同じ環境を作り直せる。
## Dockerfileの各行の役割
FROM nginx:1.29-alpine　は土台となるイメージを指定する行。Docker Hub にあるNginxのOfficial Image を使うことで、Nginx本体と起動設定を自分で用意する必要がない。
COPY index.html /user/share/nginx/html/index.html はホスト側にあるindex.htmlをイメージ内のNginxの公開ディレクトリへコピーする行である。これにより、Nginxの標準ページが自分で作ったhtmlに置き換わる。
## 動作確認
## 動作確認

`index.html` の見出しを `<h1>Docker Composeで配信中</h1>` に変更し、`docker compose up -d --build` で再ビルドした。

### 変更後の見出し

`curl http://localhost:8080` の結果に、次の見出しが含まれていることを確認した。ブラウザ（PORTSタブの8080番から `Open in Browser`）でも、見出し「Docker Composeで配信中」が表示された。

```html
<h1>Docker Composeで配信中</h1>
```

### docker compose ps の結果

```text
@4724129 ➜ /workspaces/web-eng-docker (main) $ docker compose ps
NAME                   IMAGE                COMMAND                  SERVICE   CREATED          STATUS          PORTS
web-eng-docker-web-1   web-eng-docker-web   "/docker-entrypoint.…"   web       25 seconds ago   Up 24 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
```

`PORTS` 列に `0.0.0.0:8080->80/tcp` と表示されており、ホストの8080番がコンテナの80番へ転送されていることを確認できた。