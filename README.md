# 参照先 : [How to Build a Todo App with React, TypeScript, NodeJS, and MongoDB](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/)

## API routes, Model, Controller, MongoDB
### [準備設定](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#getting-set-up)
- ``$ mkdir server``
- ターミナル移動 : ``./server``
- ``$ yarn init`` [(doc)](https://classic.yarnpkg.com/ja/docs/cli/init/#toc-yarn-init)  
  対話型の設定セッションにより``package.json``の作成
- 作成 : ``./server/.gitignore`` (node)
- ``$ yarn add typescript -g``  
  [yarn add](https://classic.yarnpkg.com/ja/docs/cli/add) : 依存関係の追加とパッケージのインストール
- ``$ yarn add express cors mongoose``
- ``$ yarn add -D @types/node @types/express @types/mongoose @types/cors``
- ``$ yarn add -D concurrently nodemon``
- ``$ tsc --init`` (tsconfig.jsonの作成)
- 修正 : ``./server/tsconfig.json`` [(commit)](https://github.com/RiSEblackbird/TS_Node_MongoDB/commit/94b787c19102c441b156b18b8f303e23584149b7)
- 修正 : ``./server/package.json`` [(commit)](https://github.com/RiSEblackbird/TS_Node_MongoDB/commit/e6d56691d47c3821de4abb2e5e023c8ba76aeca6)
### [Todoに関する定義](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#create-a-todo-type)
- 作成 : ``./server/src/types/todo.ts`` [(commit)](https://github.com/RiSEblackbird/TS_Node_MongoDB/commit/45c69c2aaad6bb5637bbaf148093881966821ff9)  
  ``mongoose``の``Document``型を拡張した``Todoインターフェース``を準備  
　　　備考(doc) : [Interfaces](https://typescript-jp.gitbook.io/deep-dive/type-system/interfaces), [extends(- MDN JS)](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Classes/extends)
- 作成 : ``./server/src/models/todo.ts`` [(commit)](https://github.com/RiSEblackbird/TS_Node_MongoDB/commit/df005a75343738800194ba0a422864d471207f11)  
  ``Todoモデル``のmongoスキーマ定義
### [APIコントローラーの作成](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#create-api-controllers)
- 作成 : src/controllers/todos/index.ts [(commit)](https://github.com/RiSEblackbird/TS_Node_MongoDB/commit/db2515936100db42a2c6a0930e09e3d3a7c6b917)  
　Read : ``getTodos`` : データの取得やレスポンス(200)の定義  
　　　備考(doc) : [req](http://expressjs.com/ja/api.html#req), [res](http://expressjs.com/ja/api.html#res)  
　Create : ``addTodo`` : 入力データをbodyオブジェクトで受け取り、Todoを作成  
　　　備考(doc) : [await](https://typescript-jp.gitbook.io/deep-dive/future-javascript/async-await)  
　Update : ``updateTodo`` : Todoの更新  
　　　備考(doc) : [Model.findByIdAndUpdate()](https://mongoosejs.com/docs/api.html#model_Model.findByIdAndUpdate)  
　Delete : ``deleteTodo`` : Todoの削除  
　　　備考(doc) : [Model.findOneAndRemove()](https://mongoosejs.com/docs/api.html#model_Model.findOneAndRemove)  
### [APIルートの作成](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#create-api-routes)
- 作成 : ``./server/src/routes/index.ts``  
  ルーティングの設定[(ルーティング - Express)](http://expressjs.com/ja/guide/routing.html)
### [サーバーの作成](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#create-a-server)
- 作成 : ``./server/src/routes/index.ts``
  MongoDBのDB設定を保持するための記述
- 作成 : ``./server/src/app.ts``  
  MongoDBとの接続  
　　　備考(doc) : [(cors - npm)](https://www.npmjs.com/package/cors), [(cors - MDN)](https://developer.mozilla.org/ja/docs/Glossary/CORS)  
　　　[app.use()](https://expressjs.com/en/guide/using-middleware.html#middleware.application) : アプリケーションレベルのミドルウェアをアプリオブジェクトのインスタンスにバインドするためのメソッド
## [Client-side with React](https://www.freecodecamp.org/news/how-to-build-a-todo-app-with-react-typescript-nodejs-and-mongodb/#client-side-with-react-and-typescript)
### 準備設定
- ターミナル移動 : ``root``  
- ``$ npx create-react-app client --template typescript``  
　　　備考(doc) : [新しい React アプリを作る – React](https://ja.reactjs.org/docs/create-a-new-react-app.html), [Selecting a template](https://create-react-app.dev/docs/getting-started#selecting-a-template)
- ターミナル移動 : ``./client``
- ``$ yarn add axios``  
　　　備考(doc) : [axios - npm](https://www.npmjs.com/package/axios), [【Ajax】axiosを使って簡単にHTTP通信 | Will Style Inc.｜神戸にあるウェブ制作会社](https://www.willstyle.co.jp/blog/2751/)
- 作成 : ``./client/src/type.d.ts``
　　　備考(doc) : [TypeScript: Handbook - Modules .d.ts](https://www.typescriptlang.org/docs/handbook/declaration-files/templates/module-d-ts.html)
- 作成 : ``./client/src/API.ts``  
　``axios``のメソッドを含みHTTPメソッドを実行する  
　　``getTodos()`` : サーバーからデータを取得する  
　　``addTodos()`` : ユーザーが入力したデータを引数として受け取り、プロミスを返す  
　　``updateTodo()`` : Todoを更新するために、更新されたデータとオブジェクトの``_id``を渡す  
　　``deleteTodo()`` : Todoの削除