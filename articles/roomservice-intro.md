---
title: "CRDTを用いた共同編集機能をHooksで使えるRoom Serviceの紹介"
emoji: "🥑"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [RoomSerive, CRDT]
published: true
---

# CRDT とは何か

Google Docs など共同編集機能があるシステムの裏で文章のコンフリクトをリアルタイムに競合解決を行っているアルゴリズムの一つに、CRDT (Conflict-free Replicated Data Type)というものがあります。

参考
https://qiita.com/everpeace/items/bb73ec64d3e682279d26

CRDT の他に、OT(Operational Transformation)、日本語では操作変換と呼ばれるものもあり、こちらは 1989 年頃には存在していて歴史が古く、Google Wave や Google Docs などで採用されています

参考
Google Wave などを開発していた中の人のブログ
「I was wrong. CRDTs are the future」という強いタイトルの記事
https://josephg.com/blog/crdts-are-the-future/

CRDT の JS 実装で有名なものに yjs があります（Star 2.8k）

yjs には CodeMirror や Monaco など主要なエディターライブラリとのバインディングに対応していて、クライアント間での通信に必要な設定も WebRTC や、WebSocket などから選択することができます
https://github.com/yjs/yjs

それらのアルゴリズムや、必要となるバックエンドを抽象化し、React Hooks で超簡単に扱えるようにしたサービスが Room Service です
https://www.roomservice.dev/

# サンプルプロジェクトを動かしてみる

```bash
yarn create next-app --example https://github.com/getroomservice/examples --example-path next.js-todolist
```

https://www.roomservice.dev/

Room Service に登録して入手した api キーを.env に追加します

```bash
echo ROOMSERVICE_API_KEY=GCuw6tBg1imHVXd0ER9cP >> .env
```

Room Service では、map と list なデータ構造を使うことができ、useMap と useList の hooks が用意されていて、このサンプルアプリでは useList を使って todo を管理しています

```tsx
const [todos, list] = useList("my-room", "todos");
```

この場合、my-room を使用している todos リストを全てのユーザーからの変更をリアルタイムに更新します。これだけです。。。

\_app.tsx では Room Service で使うセッショントークンを生成しています。

https://www.roomservice.dev/docs/auth

このサンプルアプリには使われていませんが、Room Service にはユーザーがアクティブの状態の時にだけ有効になる Presence という Hooks があります。
これはユーザーが退室するとそのデータは削除されるため、「誰が部屋にいるか」のユーザーリストや、カーソル位置の共有などに最適でそれらが簡単に実装できます！

この Presence を使ったサンプルアプリをみてみます

https://github.com/getroomservice/examples/tree/master/next.js-presence

.env に Room Service の API キーを追加して

```tsx
echo ROOMSERVICE_API_KEY=APIKEY >> .env
```

yarn dev でマウスカーソルの座標を共有するアプリが立ち上がります

<!-- ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/170becdf-8ec6-4d0e-93b1-6a00c7df7a5d/Jan-21-2021_23-01-28.gif](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/170becdf-8ec6-4d0e-93b1-6a00c7df7a5d/Jan-21-2021_23-01-28.gif) -->

不要なテキストを消してみるとマウスカーソル位置の共有に必要なコードはたったこれだけです。usePresence で my-room 内の positions という Presence を参照する hooks を生やすだけでユーザーがアクティブの時にのみ有効なデータを使えました

```tsx
import { usePresence } from "@roomservice/react";
import { useEffect } from "react";

export default function Home() {
  const [positions, positionsClient] = usePresence("my-room", "positions");

  useEffect(() => {
    window.onmousemove = (e: MouseEvent) => {
      positionsClient.set({ x: e.clientX, y: e.clientY });
    };
  }, []);

  return <div>Positions: {JSON.stringify(positions)}</div>;
}
```

最後に、useMap で textarea、usePresence でマウスカーソルがリアルタイムに
に共有されるサンプルプロジェクトを作ってみましたので参考に！

<!-- ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/48e42e7c-4fbb-4484-99db-c5d6cf548b4c/Jan-21-2021_21-39-51.gif](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/48e42e7c-4fbb-4484-99db-c5d6cf548b4c/Jan-21-2021_21-39-51.gif) -->

https://github.com/rakutek/roome-service-presence
