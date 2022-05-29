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


