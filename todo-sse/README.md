# todo-sse

## 必要環境

Node.js 14

## 環境構築

```
$ npm ci
```

## 動作確認

```
$ npm start
$ open http://localhost:3000/
```

DevTools の［Network］タブ内に/api/todos/events へのリクエストが表示され、これを選択してさらにその中の［EventStream］タブを開くと、SSE の通信の内容が以下のように表示される。

<img width="1509" alt="screenshot" src="https://github.com/shuntagami/nodejs-todo/assets/69618840/9116c782-9115-451b-b7e2-f38636e485e3">

SSE の効果を見るために、npm start を実行したのとは別のターミナルで REPL を開き、次のように POST /api/todos を実行。DevTools の［EventStream］タブで、新しく登録した ToDo を含む id 2 のメッセージがリアルタイムでブラウザに送信されていることを確認できる。

```
❯ node --experimental-repl-await
Welcome to Node.js v14.21.2.
Type ".help" for more information.
> require('isomorphic-fetch')
[Function (anonymous)]
> .editor
// Entering editor mode (Ctrl+D to finish, Ctrl+C to cancel)
await fetch('http://localhost:3000/api/todos', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ title: 'ペン入れ' })
})

Ctrl － D

Response {
  size: 0,
  timeout: 0,
  [Symbol(Body internals)]: {
    body: PassThrough {
      _readableState: [ReadableState],
      _events: [Object: null prototype],
      _eventsCount: 2,
      _maxListeners: undefined,
      _writableState: [WritableState],
      allowHalfOpen: true,
      [Symbol(kCapture)]: false,
      [Symbol(kTransformState)]: [Object]
    },
    disturbed: false,
    error: null
  },
  [Symbol(Response internals)]: {
    url: 'http://localhost:3000/api/todos',
    status: 201,
    statusText: 'Created',
    headers: Headers { [Symbol(map)]: [Object: null prototype] },
    counter: 0
  }
}
> console.log(_.status, await _.text())
201 {"id":3,"title":"ペン入れ","completed":false}
```

<img width="1509" alt="screenshot" src="https://github.com/shuntagami/nodejs-todo/assets/69618840/29136be1-8264-4e3c-a375-c059791056cd">

## ディレクトリ構成

### サーバーサイド

https://github.com/shuntagami/nodejs-todo/blob/main/todo-sse/app.js

### クライアントサイド

https://github.com/shuntagami/nodejs-todo/blob/main/todo-sse/components/Todos.js
