# _TS_Node_MongoDB

(This repository is my own self-study document
)

## リポジトリの目的

- 表題技術の基礎学習
- 自分用のリファレンス集の作成

下記のチュートリアルを参照させていただきました。

- ***[How to Build a Todo App with React, TypeScript, NodeJS, and MongoDB - freeCodeCamp](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/)***

## 全体像理解のために重要なリファレンス

　(別リポジトリで登場した参照先もこちらに集約していく)

### JavaScript

- [extends(- MDN JS)](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Classes/extends)
  - ``class ChildClass extends ParentClass { ... }``
  - [意]：``ChildClassクラス``は``ParentClassクラス``を``継承``している
- [Promiseを使う - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Using_promises)
  - 非同期処理の最終的な完了もしくは失敗を表すオブジェクトのこと
  - ``then()``関数, ``catch()``関数の出元

### TypeScript固有

### Express

### React

- [React Hooks の基本的な使い方 / Web Design Leaves](https://www.webdesignleaves.com/pr/jquery/react_basic_04.html)

### MongoDB(, mongoose)

- [Quick Start — Node.js](https://docs.mongodb.com/drivers/node/quick-start#create-a-mongodb-cluster)
  - Node.jsとの接続のためのチュートリアル
  - クラスターの建て方が解説されている
  - 今回のTodoチュートリアルでは、Mongoのインストール後に[[#Create a MongoDB Cluster]](https://docs.mongodb.com/drivers/node/quick-start#create-a-mongodb-cluster)へ続ける
- [Get Started with Atlas — MongoDB Atlas](https://docs.atlas.mongodb.com/getting-started/)
  - ``MongoDB Atlas``はMongoDBのマネージドサービス
  - ``M0``クラスターであれば無料
- [Connection String URI Format — MongoDB Manual](https://docs.mongodb.com/manual/reference/connection-string/)
  - ``mongoose.connect()``に渡す``接続文字列``の解説

## [チュートリアル](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/)の工程(適宜補完, 変更)

- ここではあくまで私自身向けにまとめています。

### API routes, Model, Controller, MongoDB

#### [準備設定](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#getting-set-up)

- ``$ mkdir server``
- ターミナル移動 : ``server``
- ``$ yarn init`` [(doc)](https://classic.yarnpkg.com/ja/docs/cli/init/#toc-yarn-init)  
  対話型の設定セッションにより``package.json``の作成
- 作成 : ``server/.gitignore`` (node)
- ``$ yarn add typescript -g``  
  [yarn add](https://classic.yarnpkg.com/ja/docs/cli/add) : 依存関係の追加とパッケージのインストール
- ``$ yarn add express cors mongoose``
- ``$ yarn add -D @types/node @types/express @types/mongoose @types/cors``
- ``$ yarn add -D concurrently nodemon``
- ``$ tsc --init`` (tsconfig.jsonの作成)
- 修正 : ``server/tsconfig.json`` [(commit)](https://github.com/RiSEblackbird/TS_Node_MongoDB/commit/94b787c19102c441b156b18b8f303e23584149b7)
- 修正 : ``server/package.json`` [(commit)](https://github.com/RiSEblackbird/TS_Node_MongoDB/commit/e6d56691d47c3821de4abb2e5e023c8ba76aeca6)

#### [Todoに関する定義](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#create-a-todo-type)

- 作成 : ``server/src/types/todo.ts`` [(commit)](https://github.com/RiSEblackbird/TS_Node_MongoDB/commit/45c69c2aaad6bb5637bbaf148093881966821ff9)  
  ``mongoose``の``Document``型を拡張した``Todoインターフェース``を準備  
　　　備考(doc) : [[Interfaces]](https://typescript-jp.gitbook.io/deep-dive/type-system/interfaces), [[extends(- MDN JS)]](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Classes/extends)
- 作成 : ``server/src/models/todo.ts`` [(commit)](https://github.com/RiSEblackbird/TS_Node_MongoDB/commit/df005a75343738800194ba0a422864d471207f11)  
  ``Todoモデル``のmongoスキーマ定義

#### [APIコントローラーの作成](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#create-api-controllers)

- 作成 : ``src/controllers/todos/index.ts`` [(commit)](https://github.com/RiSEblackbird/TS_Node_MongoDB/commit/db2515936100db42a2c6a0930e09e3d3a7c6b917)  
　Read : ``getTodos`` : データの取得やレスポンス(200)の定義  
　　　備考(doc) : [[req]](http://expressjs.com/ja/api.html#req), [[res]](http://expressjs.com/ja/api.html#res)  
　Create : ``addTodo`` : 入力データをbodyオブジェクトで受け取り、Todoを作成  
　　　備考(doc) : [[await]](https://typescript-jp.gitbook.io/deep-dive/future-javascript/async-await)  
　Update : ``updateTodo`` : Todoの更新  
　　　備考(doc) : [[Model.findByIdAndUpdate()]](https://mongoosejs.com/docs/api.html#model_Model.findByIdAndUpdate)  
　Delete : ``deleteTodo`` : Todoの削除  
　　　備考(doc) : [[Model.findOneAndRemove()]](https://mongoosejs.com/docs/api.html#model_Model.findOneAndRemove)  

#### [APIルートの作成](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#create-api-routes)

- 作成 : ``server/src/routes/index.ts``  
  ルーティングの設定[(ルーティング - Express)](http://expressjs.com/ja/guide/routing.html)

#### [サーバーの作成](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#create-a-server)

- 作成 : ``server/src/routes/index.ts``
  MongoDBのDB設定を保持するための記述
- アプリケーションと接続するMongoDBのクラスターをセッティングする
- 作成 : ``server/src/app.ts``  
  MongoDBとの接続  
  　[app.use()](https://expressjs.com/en/guide/using-middleware.html#middleware.application) : アプリケーションレベルのミドルウェアをアプリオブジェクトのインスタンスにバインドするためのメソッド  
　　　備考(doc) : [[cors - npm]](https://www.npmjs.com/package/cors), [[cors - MDN]](https://developer.mozilla.org/ja/docs/Glossary/CORS)  

#### JSへのコンパイル

- $ npx tsc
  - ``dist``ディレクトリにJavaScriptにコンパイルされたファイルが出力される
　　　
### [Client-side with React](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#client-side-with-react-and-typescript)

#### 準備設定

- ターミナル移動 : ``root``  
- ``$ npx create-react-app client --template typescript``  
　　　備考(doc) : [[新しい React アプリを作る – React]](https://ja.reactjs.org/docs/create-a-new-react-app.html), [[Selecting a template]](https://create-react-app.dev/docs/getting-started#selecting-a-template)
- ターミナル移動 : ``client``
- ``$ yarn add axios``  
　　　備考(doc) : [[axios - npm]](https://www.npmjs.com/package/axios), [[【Ajax】axiosを使って簡単にHTTP通信 | Will Style Inc.｜神戸にあるウェブ制作会社]](https://www.willstyle.co.jp/blog/2751/)
- 作成 : ``client/src/type.d.ts``  
　　　備考(doc) : [[TypeScript: Handbook - Modules .d.ts]](https://www.typescriptlang.org/docs/handbook/declaration-files/templates/module-d-ts.html)
- 作成 : ``client/src/API.ts``  
　``axios``のメソッドを含みHTTPメソッドを実行する  
　　``getTodos()`` : サーバーからデータを取得する  
　　``addTodos()`` : ユーザーが入力したデータを引数として受け取り、プロミスを返す  
　　``updateTodo()`` : Todoを更新するために、更新されたデータとオブジェクトの``_id``を渡す  
　　``deleteTodo()`` : Todoの削除

### [Create the components](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#create-the-components)

- $ mkdir src/components
- 作成 : ``client/src/components/AddTodo.tsx``  
　Todoの投稿フォーム  
　JS/Reactの基本事項  
　　[``useState``](https://ja.reactjs.org/docs/hooks-reference.html#usestate) : 現在の state の値と、それを更新するための関数とをペアにして返す  
　　[``スプレッド構文(...) - MDN``](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Spread_syntax) : 配列要素を複製する  
　　``interface FormEvent`` : ``SyntheticEvent<T>``を継承  
　　　[合成イベント (SyntheticEvent) – React](https://ja.reactjs.org/docs/events.html)  
　　[``onChange``](https://ja.reactjs.org/docs/dom-elements.html#onchange) : フォームコントロールの値の変更を検知(changeイベント)  
　　[``className``](https://ja.reactjs.org/docs/dom-elements.html#classname) : CSSクラスの指定  
　　　備考(doc) : [[フック早わかり – React]](https://ja.reactjs.org/docs/hooks-overview.html#state-hook), [[GlobalEventHandlers.onchange - Web API | MDN]](https://developer.mozilla.org/ja/docs/Web/API/GlobalEventHandlers/onchange), [[フォーム – React]](https://ja.reactjs.org/docs/forms.html), [[DOM 要素 – React]](https://ja.reactjs.org/docs/dom-elements.html)
- 作成 : ``client/src/components/TodoItem.tsx`` 
　Todoの操作フォーム  
　　　備考(doc) : [[レンダープロップ – React]](https://ja.reactjs.org/docs/render-props.html), [[コンポーネントと props – React]](https://ja.reactjs.org/docs/components-and-props.html)
- 編集 : ``client/src/App.tsx``  
  - [編集1]() : 各インポート -> Reactとそのフック、コンポーネント、APIのCRUD用メソッド (フックについては別途よく調べる)  
  　　　備考(doc) : [[useEffect - React]](https://ja.reactjs.org/docs/hooks-reference.html#useeffect), [[副作用フックの利用法 – React]](https://ja.reactjs.org/docs/hooks-effect.html)
  - [編集2]() : ``handleSaveTodo`` : ``addTodo()``でAxiosが'201'ステータスを返した場合にエラーを表示する
  - [編集3]() : 上記同様、更新と削除
  - [編集4]() : 描画部分

#### 起動方法

- ``server``, ``client``の両方で``$ yarn start``

### Errors

- ``UnhandledPromiseRejectionWarning: MongoError: Authentication failed.``
  - ``app.ts``の接続文字列の設定方法を知らず、チュートリアル著者の設定を引用していたため発生
  - ``MongoDB Atlas``で自身のクラスターをセッティングして、接続文字列を修正したことで解消した
- ``UnhandledPromiseRejectionWarning: TypeError: Cannot read property 'name' of undefined``
  - フォームからTodoをPostする際に発生 <--

## 階層

### client

~~~
├── node_modules
├── public
├── src
|  ├── API.ts
|  ├── App.test.tsx
|  ├── App.tsx
|  ├── components
|  |  ├── AddTodo.tsx
|  |  └── TodoItem.tsx
|  ├── index.css
|  ├── index.tsx
|  ├── react-app-env.d.ts
|  ├── setupTests.ts
|  └── type.d.ts
├── tsconfig.json
├── package.json
└── yarn.lock
~~~

### server

~~~
├── dist
├── node_modules
├── src
   ├── app.ts
   ├── controllers
   |  └── todos
   |     └── index.ts
   ├── models
   |  └── todo.ts
   ├── routes
   |  └── index.ts
   └── types
      └── todo.ts
├── nodemon.json
├── package.json
├── tsconfig.json
~~~
