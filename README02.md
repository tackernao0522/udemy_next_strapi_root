## 17. _app.jsの中身を1から記述してみる

+ `root $ touch frontend/pages/_app.js`を実行<br>

+ `frontend/pages/_app.js`を編集<br>

```js:_app.js
import React from "react"
import App from "next/app"
import Head from "next/Head"

export default class MyApp extends App {
  render() {
    const { Component, pageProps } = this.props
    return (
      <>
        <Head></Head>
        <Component {...pageProps} />
      </>
    )
  }
}
```

## 18. CSSライブラリのreactstrapをインストールする

+ `root $ docker compose run --rm frontend yarn add reactstrap`を実行<br>

+ `frontend/pages/_app.js`を編集<br>

```js:_app.js
import React from "react"
import App from "next/app"
import Head from "next/Head"

export default class MyApp extends App {
  render() {
    const { Component, pageProps } = this.props
    return (
      <>
        <Head>
          <link
            rel="stylesheet"
            href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css"
          />
        </Head>
        <Component {...pageProps} />
      </>
    )
  }
}
```

+ `frontend/pages/index.js`を編集<br>

```js:index.js
import { Alert } from "reactstrap"

const index = () => {
  return (
    <div>
      <div>
        <Alert color="primary">Hello Project</Alert>
        <Button color="primary">Hello from nexjs</Button>
      </div>
    </div>
  );
}

export default index;
```

## 19 ページ共通部分のLayoutコンポーネントを作成する

+ `root $ mkdir front/components && touch $_/Layout.js`を実行<br>

+ `front/components/Layout.js`を編集<br>

+ `nafe`を実行<br>

```js:Layout.js
import React from "react"
import App from "next/app"
import Head from "next/Head"
import Link from "next/link"
import { Container, Nav, NavItem } from "reactstrap";

const Layout = (props) => {
  return (
    <div>
      <Head>
        <title>フードデリバリーサービス</title>
        <link
          rel="stylesheet"
          href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css"
        />
      </Head>
      <header>
        <Nav className="navbar navbar-dark bg-dark">
          <NavItem>
            <Link href="/">
              <a className="navbar-brand">ホーム</a>
            </Link>
          </NavItem>
        </Nav>
      </header>
      <Container>{props.children}</Container>
    </div>
  );
}

export default Layout;
```

+ `frontend/pages/_app.js`を編集<br>

```js:_app.js
import React from "react"
import App from "next/app"
import Head from "next/Head"
import Layout from "../components/Layout"

export default class MyApp extends App {
  render() {
    const { Component, pageProps } = this.props
    return (
      <>
        <Head>
          <link
            rel="stylesheet"
            href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css"
          />
        </Head>
        <Layout>
          <Component {...pageProps} />
        </Layout>
      </>
    )
  }
}
```

+ `frontend/components/Layout.js`を編集<br>

```js:Layout.js
import React from "react"
import App from "next/app"
import Head from "next/Head"
import Link from "next/link"
import { Container, Nav, NavItem } from "reactstrap";

const Layout = (props) => {
  return (
    <div>
      <Head>
        <title>フードデリバリーサービス</title>
        <link
          rel="stylesheet"
          href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css"
        />
      </Head>
      <header>
        <style jsx>
          {`
            a {
              color: white
            }
          `}
        </style>
        <Nav className="navbar navbar-dark bg-dark">
          <NavItem>
            <Link href="/">
              <a className="navbar-brand">ホーム</a>
            </Link>
          </NavItem>
          <NavItem className="ml-auto">
            <Link href="/login">
              <a className="nav-link">サインイン</a>
            </Link>
          </NavItem>
          <NavItem>
            <Link href="/register">
              <a className="nav-link">サインアップ</a>
            </Link>
          </NavItem>
        </Nav>
      </header>
      <Container>{props.children}</Container>
    </div>
  );
}

export default Layout;
```

## strapiを3.6.8に構築し直し

https://lon.sagisawa.me/2021/07/running-strapi <br>

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

+ http://localhost:1337/admin にアクセス

## レストランのデータを作成する

+ `Content-Types-Builder`をクリック<br>

+ `Create new collection type`をクリック<br>

+ `Display name`に`restaurant`を入力して`Continue`をクリック<br>

+ `Text`をクリック<br>

+ `Name`に`name`を入力して`Add another field`をクリック<br>

+ `Rich text`をクリックして`Name`に`description`を入力して`Add another field`をクリック<br>

+ `Media`をクリックして`Name`に`image`を入力して`Finish`をクリック<br>

+ `Save`ボタンをクリック<br>

+ 左の`COLLECTION TYPES`の`Restaurant`をクリック<br>

+ 右上の`Add New Restaurants`をクリック<br>

+ `Name`に`Italian Restaurant`を入力<br>

+ `Description`に`イタリアンのレストランです。`と入力<br>

+ `Image`の画像(3枚)をアップロードする<br>

+ `Upload3 aseets to the library`をクリック<br>

+ 1枚目の画像のみ選択して`Finish`をクリック<br>

+ `Save`をクリック<br>

+ 繰り返し3件入れてみる<br>

+ それぞれ`Publish`をクリックする<br>

## 24 レストランのデータを取得できるか確認する

+ Strapi ダッシュボードの左の`Settings`をクリック<br>

+ `USERS & PERMISSIONS PLUGIN`の`Roles`をクリック<br>

+ `Public`をクリック<br>

+ `Application`の`RESTURANT`の中の`find`と`findone`にチェックを入れて`Save`する<br>

+ `http://localhost:1337/restaurants`にアクセスしてみる<br>

## 25 GraphQLプラグインをインストールして利用する

+ `backend $ touch .yarnrc`を実行<br>

+ `backend/.yarnrc`を編集<br>

```:.yanrc
network-timeout 600000
```
+ `root docker compose run --rm backend yarn strapi install graphql`を実行<br>

+ http://localhost:1337/graphql`にアクセスする<br>

+ `root $ docker compose run --rm frontend yarn add @apollo/react-hooks@3.1.5 @apollo/react-ssr@3.1.5 apollo-boost@0.4.7 apollo-link-http@1.5.17 graphql@15.0.0 next-apollo@3.1.10`を実行<br>

## 27 GraphQLが使えるようにサーバー設定する

+ `frontend $ mkdir lib && touch $_/apollo.js`を実行<br>

+ `frontend/lib/apollo.js`を編集<br>

```js:apollo.js
import { HttpLink } from "apollo-link-http"
import { withData } from "next-apollo"

const API_URL = process.env.NEXT_PUBLIC_API_URL || "http;//localhost:1337";

const config = {
  link: new HttpLink({
    uri: `${API_URL}/graphql`
  })
}

export default withData(config)
```

+ `frontend/pages/_app.js`を編集<br>

```js:_app.js
import React from "react"
import App from "next/app"
import Head from "next/Head"
import Layout from "../components/Layout"
import withData from "../lib/apollo" // 追加

// 編集
class MyApp extends App {
  render() {
    const { Component, pageProps } = this.props
    return (
      <>
        <Head>
          <link
            rel="stylesheet"
            href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css"
          />
        </Head>
        <Layout>
          <Component {...pageProps} />
        </Layout>
      </>
    )
  }
}

export default withData(MyApp) // 追加
```

+ `frontend/pages/index.js`を編集<br>

```js:index.js
import { Col, Input, InputGroup, InputGroupText, Row } from "reactstrap"

const index = () => {
  return (
    <div className="container-fluid">
      <Row>
        <Col>
          <div className="search">
            <InputGroup>
              <InputGroupText>探す</InputGroupText>
              <Input placeholder="レストラン名を入力してください" />
            </InputGroup>
          </div>
        </Col>
      </Row>
      <style jsx>{`
        .search {
          margin: 20px;
          width: 500px;
        }
      `}
      </style>
    </div>
  );
}

export default index;
```

## 29 レストランカードのコンポーネントを作成

+ `frontend $ mkdir components/RestaurantsList && touch $_/index.js`を実行<br>

(nafe)<br>

+ `frontend/components/RestaurantsList/index.js`を編集<br>

```js:index.js
import { Row } from "reactstrap";

const RestaurantList = () => {
  return (
    <Row>
      Hello
    </Row>
  );
}

export default RestaurantList;
```

+ `frontend/pages/index.js`を編集<br>

```js:index.js
import { Col, Input, InputGroup, InputGroupText, Row } from "reactstrap"
import RestaurantList from "../components/RestauranstList";

const index = () => {
  return (
    <div className="container-fluid">
      <Row>
        <Col>
          <div className="search">
            <InputGroup>
              <InputGroupText>探す</InputGroupText>
              <Input placeholder="レストラン名を入力してください" />
              // 追加
            </InputGroup>
          </div>
          <RestaurantList />
        </Col>
      </Row>
      <style jsx>{`
        .search {
          margin: 20px;
          width: 500px;
        }
      `}
      </style>
    </div>
  );
}

export default index;
```

+ `frontend/components/RestaurantsList/index.js`を編集<br>

```js:index.js
import Link from "next/link";
import { Card, CardBody, CardImg, CardTitle, Col, Row } from "reactstrap";

const RestaurantList = () => {
  return (
    <Row>
      <Col>
        <Card>
          <CardImg src="" />
          <CardBody>
            <CardTitle>Italian Restaurant</CardTitle>
            <CardTitle>イタリアンのレストランです。</CardTitle>
          </CardBody>
          <div className="card-footer">
            <Link
              href="/restaurants?id=1"
              as="/restaurants/1"
            >
              <a className="btn btn-primary">もっと見る</a>
            </Link>
          </div>
        </Card>
      </Col>
    </Row>
  );
}

export default RestaurantList;
```
