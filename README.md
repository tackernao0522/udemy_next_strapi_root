# セクション 2: Strapi 入門

## Strapi の構築

https://qiita.com/teixan/items/037ba91e37b936ed378a<br>

- `root $ touch docker-compose.yml`を実行<br>

- `root/docker-compose.ymlを編集`<br>

```yml:docker-compose.yml
version: '3.8'

services:
  postgres:
    image: postgres:13.1-alpine
    container_name: 'strapi-practice-db'
    ports:
      - 5432:5432
    environment:
      POSTGRES_PASSWORD: $POSTGRES_PASSWORD
    volumes:
      - ./db/pgsql-data:/var/lib/postgresql/data
  app:
    image: node:16.13.1
    ports:
      - '8080:8080'
      - '1337:1337'
    volumes:
      - ./app:/src
    working_dir: /src/strapi-practice
    tty: true
    depends_on:
      - postgres
    command: yarn develop
```

- `root $ docker-compose up -d`を実行<br>

- http://localhost:1337/admin/ にアクセスしてログインする<br>

- `Create your first Content type`をクリック<br>

## 5 食べ物のコレクションを作成してみる

- `COLLECTION TYPES`の`User`を選択して確認してみる<br>

- `User`の下の `Create new collection type`をクリックする<br>

- `Configrations`の`Display name`に`food`と入力して`Continue`を押下する<br>

- `Text`をクリック<br>

- `Name`に`name`を入力 `Type`は`Short text`を入力して`ADVANCED SETTINGS`をクリック<br>

- `Minimun length`にチェックを入れ `2`を入れてみる<br>

- `Maximum length`にチェックを入れ `20`を入れてみる<br>

- `Required field`にチェックを入れる(入力を必須カラムにするかしないか)<br>

- `Add another field`を押下<br>

- `Text`をクリック<br>

- `Name`に`description`を入力<br>

- `Type`は`Short text`を選択<br>

- `Add anoter field`を押下<br>

- `Number`をクリック<br>

- `Name`に`price`を入力<br>

- `Number format`に`integer (ex: 10)`を選択<br>

- `Finish`を押下<br>

- `Save`ボタンを押下<br>

- `Content Manager`をクリック<br>

- 真ん中の`+ Create new entry`をクリック<br>

## 6 具体的な食べ物のデータを入力してみる<br>

- `name`に`りんご`を入力<br>

- `description`に`赤い果物です。`を入力<br>

- `price`に`100`を入れる<br>

- `Save`ボタンを押下<br>

- 右上の`Publish`を押下<br>

- `Create new entry`で他の食べ物も入れてみる<br>

## 7 　登録した食べ物データを GET メソッドで取得してみる<br>

- 左の`Settings`をクリック<br>

- `USERS & PERMISSIONS PLUGIN`の`Roles`をクリック<br>

- リスト`Public`をクリック<br>

- `Permissions`の `Food`をプルダウンする<br>

- `Food`は検索するだけにするので`find`と`findOne`にチェックを入れる<br>

- 右側に`GET`メソッドのエンドポイントが出来上がっている `/api/foods/:id`<br>

- `Save`ボタンを押下<br>

- localhost:1337/api/foods/ にアクセスしてみる<br>

```:browser
{
data: [
{
id: 1,
attributes: {
name: "りんご",
description: "赤い果物です。",
price: 100,
createdAt: "2022-05-28T02:13:35.058Z",
updatedAt: "2022-05-28T02:14:49.796Z",
publishedAt: "2022-05-28T02:14:49.787Z"
}
},
{
id: 2,
attributes: {
name: "みかん",
description: "オレンジの果物です。",
price: 150,
createdAt: "2022-05-28T02:17:06.869Z",
updatedAt: "2022-05-28T02:17:09.860Z",
publishedAt: "2022-05-28T02:17:09.849Z"
}
},
{
id: 3,
attributes: {
name: "ぶどう",
description: "グレープです。",
price: 200,
createdAt: "2022-05-28T02:17:57.015Z",
updatedAt: "2022-05-28T02:17:58.591Z",
publishedAt: "2022-05-28T02:17:58.586Z"
}
}
],
meta: {
pagination: {
page: 1,
pageSize: 25,
pageCount: 1,
total: 3
}
}
}
```

## 10　実際にPostmanを使ってAPIテストを始める

+ `Postman(GET) http://localhost:1337/api/foods`にアクセスして`Send`<br>

```:results
{
    "data": [
        {
            "id": 1,
            "attributes": {
                "name": "りんご",
                "description": "赤い果物です。",
                "price": 100,
                "createdAt": "2022-05-28T02:13:35.058Z",
                "updatedAt": "2022-05-28T02:14:49.796Z",
                "publishedAt": "2022-05-28T02:14:49.787Z"
            }
        },
        {
            "id": 2,
            "attributes": {
                "name": "みかん",
                "description": "オレンジの果物です。",
                "price": 150,
                "createdAt": "2022-05-28T02:17:06.869Z",
                "updatedAt": "2022-05-28T02:17:09.860Z",
                "publishedAt": "2022-05-28T02:17:09.849Z"
            }
        },
        {
            "id": 3,
            "attributes": {
                "name": "ぶどう",
                "description": "グレープです。",
                "price": 200,
                "createdAt": "2022-05-28T02:17:57.015Z",
                "updatedAt": "2022-05-28T02:17:58.591Z",
                "publishedAt": "2022-05-28T02:17:58.586Z"
            }
        }
    ],
    "meta": {
        "pagination": {
            "page": 1,
            "pageSize": 25,
            "pageCount": 1,
            "total": 3
        }
    }
}
```

+ `Postman(POST) `http://localhost:1337/api/foods`を入力<br>

+ `Body`タブを選択して`raw`タブを選択して`JSON`を選択して下記を記述する<br>

```:json
{
    "data": {
        "name": "メロン",
        "description": "まん丸のメロンです。",
        "price": 1000
    }
}
```

+ この状態で`Send`しても下記の結果になる<br>

```:results
{
    "data": null,
    "error": {
        "status": 403,
        "name": "ForbiddenError",
        "message": "Forbidden",
        "details": {}
    }
}
```

## 11 Strapiにユーザー追加と権限を与える

+ `Content Manager`をクリックして`COLLECTION TYPES`の`User`をクリック<br>

+ `Create new entry`をクリック<br>

+ `username`に`takaki`を入力<br>

+ `email`に`takaki55730317@gmail.com`を入力<br>

+ `password`に`password`を入力<br>

+ `confirmed`を`true`にしておく<br>

+ `blocked`は`false`にしておく<br>

+ `Save`ボタンを押下<br>

+ `Settings`をクリック<br>

+ `USERS & PERMISSION PLUGIN`の`Roles`をクリック<br>

+ `Autenticated`をクリック<br>

+ `Permissions`の`Food`をプルダウンして`Select all`にチェックを入れる<br>

+ `Save`ボタンを押下<br>

+ `Postman(POST) http://localhost:1337/api/auth/local`で`Send`<br>


```:results
{
    "data": null,
    "error": {
        "status": 400,
        "name": "ValidationError",
        "message": "2 errors occurred",
        "details": {
            "errors": [
                {
                    "path": [
                        "identifier"
                    ],
                    "message": "identifier is a required field",
                    "name": "ValidationError"
                },
                {
                    "path": [
                        "password"
                    ],
                    "message": "password is a required field",
                    "name": "ValidationError"
                }
            ]
        }
    }
}
```

+ `Postmanの`Body`の`raw`の`JSON`に下記のように記述<br>

```:json
{
    "identifier": "takaki",
    "password": "password"
}
```

+ `Postman Send`をクリック<br>

```results
{
    "jwt": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjUzNzA5NjYzLCJleHAiOjE2NTYzMDE2NjN9.iUOuiN1QBLEtnymD05fsGBgoz__P2iFxkQG2PaOlCsQ",
    "user": {
        "id": 1,
        "username": "takaki",
        "email": "takaki55730317@gmail.com",
        "provider": "local",
        "confirmed": true,
        "blocked": false,
        "createdAt": "2022-05-28T03:37:10.422Z",
        "updatedAt": "2022-05-28T03:37:10.422Z"
    }
}
```

## 12 Postmanを使って新しい食べ物を追加する作業を始める

+ `jwt`の値をコピーしておく<br>

+ `POST FOOD`を選択して`Authorization`を選択して`Type`を`Bearer Token`にする<br>

+ `Token`にコピーした`jwt`の値を貼り付けて`Send`する<br>

```:results
{
    "data": {
        "id": 4,
        "attributes": {
            "name": "メロン",
            "description": "まん丸のメロンです。",
            "price": 1000,
            "createdAt": "2022-05-28T04:02:32.215Z",
            "updatedAt": "2022-05-28T04:02:32.215Z",
            "publishedAt": "2022-05-28T04:02:32.210Z"
        }
    },
    "meta": {}
}
```

#　セクション3: Next.jsでショッピングサイトを構築(Next.jsセットアップ編)

+ `mkdir fontend && touch $_/Dockerfile`を実行<br>

+ `frontend/Dockerfile`を編集<br>

```:Dockerfile
FROM node:16.13.1-alpine

ARG WORKDIR
ARG API_URL

ENV HOME=/${WORKDIR} \
  LANG=C.UTF-8 \
  TZ=Asia/Tokyo \
  HOST=0.0.0.0 \
  API_URL=${API_URL}

WORKDIR ${HOME}

# ローカル上のpackageをコンテナにコピーしてインストールする
COPY package*.json ./
RUN yarn install

# コンテナにNextプロジェクトをコピー
COPY . ./
```

+ `root/docker-compose.yml`を編集<br>

```yml:docker-compose.yml
version: "3.8"

services:
  postgres:
    image: postgres:13.1-alpine
    container_name: "strapi-backend-db"
    ports:
      - 5432:5432
    command: postgres -c stats_temp_directory=/tmp
    environment:
      POSTGRES_PASSWORD: $POSTGRES_PASSWORD
    volumes:
      - ./backend/db/pgsql-data:/var/lib/postgresql/data
  backend:
    image: node:16.13.1
    ports:
      - "8080:8080"
      - "$API_PORT:1337"
    volumes:
      - ./backend:/src
    working_dir: /src/api-data
    tty: true
    depends_on:
      - postgres
    command: yarn develop

  frontend:
      build:
        context: ./frontend
        args:
          WORKDIR: $WORKDIR
          # API_URL: "http://localhost:$API_PORT" # 追加
      # コンテナで実行したいコマンド(CMD)
      command: npm run dev
      volumes:
        - "./frontend:/$WORKDIR"
      ports:
        - "$FRONT_PORT:3000"
      depends_on:
        - backend
```

+ `docker compose run --rm frontend yarn add next@9.4.0 react@16.13.1 react-dom@16.13.1`を実行<br>

+ `frontend/package.json`を編集<br>

```json:package.json
{
  "scripts": {
    "dev": "next",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "9.4.0",
    "react": "16.13.1",
    "react-dom": "16.13.1"
  }
}
```

+ `frontend touch .gitignore`を実行<br>

+ `frontend/.gitignore`を編集<br>

```:.gitignore
.next
node_modules
```

+ `frontend $ touch .env.development`を実行<br>

+ `frontend/.env.devlopment`を編集<br>

```
NEXT_PUBLIC_API_URL="http://localhost:1337"
```

+ `root $ docker compose up frontend`を実行<br>

+ `root $ mkdir frontend/pages && touch $_/index.js`を実行<br>

+ `frontend/pages/index.js`を編集<br>

```js:index.js
const index = () => {
  return (
    <div>
      Hello Next.js
    </div>
  );
}

export default index;
```

+ `localhost:3000`にアクセスしてみる<br>
