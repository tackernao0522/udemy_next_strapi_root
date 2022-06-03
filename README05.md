# セクション 5: Next.js でショッピングサイトを構築(ユーザー登録・ログイン編)

## 42 ユーザーアカウント登録に必要なライブラリ群をインストールする

参考: https://docs-v3.strapi.io/developer-docs/latest/development/plugins/users-permissions.html#registration <br>

+ `root $ docker compose run --rm forntend yarn add axios@0.19.2 js-cookie@2.2.1 isomorphic-fetch@2.2.1` を実行<br>

## 43 アカウント登録用のページを作成する

+ `root $ touch frontend/pages/register.js`を実行<br>

(nafe) <br>

+ `frontend/pages/register.js`を編集<br>

```js:register
import { Button, Col, Container, Form, FormGroup, Input, Label, Row } from "reactstrap";

const register = () => {
  return (
    <Container>
      <Row>
        <Col>
          <div className="paper">
            <div className="header">
              <h2>ユーザー登録</h2>
            </div>
          </div>
          <section className="wrapper">
            <Form>
              <fieldset>
                <FormGroup>
                  <Label>ユーザー名：</Label>
                  <Input
                    type="text"
                    name="username"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <FormGroup>
                  <Label>メールアドレス：</Label>
                  <Input
                    type="email"
                    name="email"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <FormGroup>
                  <Label>パスワード：</Label>
                  <Input
                    type="password"
                    name="password"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <span>
                  <a href="">
                    <small>パスワードをお忘れですか？</small>
                  </a>
                </span>
                <Button style={{ float: "right", width: 120 }} color="primary">
                  登録
                </Button>
              </fieldset>
            </Form>
          </section>
        </Col>
      </Row>
    </Container>
  );
}

export default register;
```

## 44 アカウント登録ページをCSSでスタイリングする

+ `frontend/pages/register.js`を編集<br>

```js:register.js
import { Button, Col, Container, Form, FormGroup, Input, Label, Row } from "reactstrap";

const register = () => {
  return (
    <Container>
      <Row>
        <Col>
          <div className="paper">
            <div className="header">
              <h2>ユーザー登録</h2>
            </div>
          </div>
          <section className="wrapper">
            <Form>
              <fieldset>
                <FormGroup>
                  <Label>ユーザー名：</Label>
                  <Input
                    type="text"
                    name="username"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <FormGroup>
                  <Label>メールアドレス：</Label>
                  <Input
                    type="email"
                    name="email"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <FormGroup>
                  <Label>パスワード：</Label>
                  <Input
                    type="password"
                    name="password"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <span>
                  <a href="">
                    <amall>パスワードをお忘れですか？</amall>
                  </a>
                </span>
                <Button style={{ float: "right", width: 120 }} color="primary">
                  登録
                </Button>
              </fieldset>
            </Form>
          </section>
        </Col>
      </Row>
      // 追加
      <style jsx>
        {`
          .paper {
            text-align: center;
            margin-top: 50px;
          }
          .header {
            width: 100%;
            margin-bottom: 30px;
          }
          .wrapper {
            padding: 10px 30px 20px 30px;
          }
        `}
      </style>
      // ここまで
    </Container>
  );
}

export default register;
```

## 45 アカウント登録やログイン用のファイルを作成する

参考: https://docs-v3.strapi.io/developer-docs/latest/development/plugins/users-permissions.html#concept <br>

参考: https://www.npmjs.com/package/js-cookie <br>

+ `root $ touch frontend/lib/auth.js`を実行<br>

+ `frontend/lib/auth.js`を編集<br>

```js:auth.js
import axios from "axios"
import Cookie from "js-cookie"

const API_URL = process.env.NEXT_PUBLIC_API_URL || "http;//localhost:1337";

// 新しいユーザーを登録
export const registerUser = async (username, email, password) => {
  await axios.post(`${API_URL}/auth/local/register`, {
    username,
    email,
    password
  })
    .then((res) => {
      Cookie.set("token", res.data.jwt, { expires: 7 })
      console.log(res.data.jwt)
    })
    .catch((err) => {
      console.log(err)
    })
}
```

## 46 ユーザー状態をグローバルに監視する

+ `root $ mkdir frontend/context && touch $_/AppContext.js`を実行<br>

+ `frontend/context/AppContext.js`を編集<br>

```js:AppContext.js
import React, { createContext } from "react"

const AppContext = createContext()

export default AppContext
```

+ `frontend/pages/_app.js`を編集<br>

```js:_app.js
import React from "react"
import App from "next/app"
import Head from "next/Head"
import Layout from "../components/Layout"
import withData from "../lib/apollo"
import AppContext from "../context/AppContext" // 追加

class MyApp extends App {
  // const [state, setState] = useState(null) と同じものをclassコンポーネントでは下記の書き方になる

  // 追加
  state = {
    user: null,
  }

  setUser = (user) => {
    this.setState({ user })
  }
  // ここまで

  render() {
    const { Component, pageProps } = this.props
    return (
      <AppContext.Provider value={{ user: this.state.user, setUser: this.setUser }}> // 追加
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
      </AppContext.Provider> // 追加
    )
  }
}

export default withData(MyApp)
```

## 47 実際にアカウント登録の関数を使う(その1)

+ `fontend/pages/register.js`を編集<br>

```js:register.js
import { useState } from "react";
import { Button, Col, Container, Form, FormGroup, Input, Label, Row } from "reactstrap";
import { registerUser } from "../lib/auth";

const register = () => {
  const [data, setData] = useState({ username: "", email: "", password: "" }) // 追加

  // 追加
  const handleRegister = () => {
    registerUser()
  }

  console.log(data)

  return (
    <Container>
      <Row>
        <Col>
          <div className="paper">
            <div className="header">
              <h2>ユーザー登録</h2>
            </div>
          </div>
          <section className="wrapper">
            <Form>
              <fieldset>
                <FormGroup>
                  <Label>ユーザー名：</Label>
                  <Input
                    type="text"
                    name="username"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                    onChange={(e) => setData({ ...data, username: e.target.value })} // 追加
                  />
                </FormGroup>
                <FormGroup>
                  <Label>メールアドレス：</Label>
                  <Input
                    type="email"
                    name="email"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                    onChange={(e) => setData({ ...data, email: e.target.value })} // 追加
                  />
                </FormGroup>
                <FormGroup>
                  <Label>パスワード：</Label>
                  <Input
                    type="password"
                    name="password"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                    onChange={(e) => setData({ ...data, password: e.target.value })} // 追加
                  />
                </FormGroup>
                <span>
                  <a href="">
                    <small>パスワードをお忘れですか？</small>
                  </a>
                </span>
                <Button
                  style={{ float: "right", width: 120 }}
                  color="primary"
                  onClick={() => { handleRegister() }} // 追加
                >
                  登録
                </Button>
              </fieldset>
            </Form>
          </section>
        </Col>
      </Row>
      <style jsx>
        {`
          .paper {
            text-align: center;
            margin-top: 50px;
          }
          .header {
            width: 100%;
            margin-bottom: 30px;
          }
          .wrapper {
            padding: 10px 30px 20px 30px;
          }
        `}
      </style>
    </Container>
  );
}

export default register;
```

## 48 実際にアカウント登録の関数を使う(その2)

+ `frontend/pages/register.js`を編集<br>

```js:register.js
import { useContext, useState } from "react";
import { Button, Col, Container, Form, FormGroup, Input, Label, Row } from "reactstrap";
import AppContext from "../context/AppContext";
import { registerUser } from "../lib/auth";

const register = () => {
  const appContext = useContext(AppContext)
  const [data, setData] = useState({ username: "", email: "", password: "" })

  const handleRegister = () => {
    registerUser(data.username, data.email, data.password)
      .then((res) => {
        appContext.setUser(res.data.user)
      })
      .catch((err) => console.log(err))
  }

  // console.log(data)

  return (
    <Container>
      <Row>
        <Col>
          <div className="paper">
            <div className="header">
              <h2>ユーザー登録</h2>
            </div>
          </div>
          <section className="wrapper">
            <Form>
              <fieldset>
                <FormGroup>
                  <Label>ユーザー名：</Label>
                  <Input
                    type="text"
                    name="username"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                    onChange={(e) => setData({ ...data, username: e.target.value })}
                  />
                </FormGroup>
                <FormGroup>
                  <Label>メールアドレス：</Label>
                  <Input
                    type="email"
                    name="email"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                    onChange={(e) => setData({ ...data, email: e.target.value })}
                  />
                </FormGroup>
                <FormGroup>
                  <Label>パスワード：</Label>
                  <Input
                    type="password"
                    name="password"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                    onChange={(e) => setData({ ...data, password: e.target.value })}
                  />
                </FormGroup>
                <span>
                  <a href="">
                    <small>パスワードをお忘れですか？</small>
                  </a>
                </span>
                <Button
                  style={{ float: "right", width: 120 }}
                  color="primary"
                  onClick={() => { handleRegister() }}
                >
                  登録
                </Button>
              </fieldset>
            </Form>
          </section>
        </Col>
      </Row>
      <style jsx>
        {`
          .paper {
            text-align: center;
            margin-top: 50px;
          }
          .header {
            width: 100%;
            margin-bottom: 30px;
          }
          .wrapper {
            padding: 10px 30px 20px 30px;
          }
        `}
      </style>
    </Container>
  );
}

export default register;
```

+ ユーザー登録をしてみてStrapiに反映されているか確認<br>

