## 50. ログイン用コンポーネントを作成する

参考: https://docs-v3.strapi.io/developer-docs/latest/development/plugins/users-permissions.html#registration (ログイン)<br>

+ `root $ touch frontend/pages/login.js`を実行<br>

+ `frontend/pages/login.js`を編集<br>

```js:login.js
import { useContext, useState } from "react";
import { Button, Col, Container, Form, FormGroup, Input, Label, Row } from "reactstrap";
import AppContext from "../context/AppContext";

const Login = () => {
  const appContext = useContext(AppContext)
  const [data, setData] = useState({ identifier: "", password: "" })

  const handleLogin = () => { }

  // console.log(data)

  return (
    <Container>
      <Row>
        <Col>
          <div className="paper">
            <div className="header">
              <h2>ログイン</h2>
            </div>
          </div>
          <section className="wrapper">
            <Form>
              <fieldset>
                <FormGroup>
                  <Label>メールアドレス：</Label>
                  <Input
                    type="email"
                    name="identifier"
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
                <Button
                  style={{ float: "right", width: 120 }}
                  color="primary"
                  onClick={() => { handleLogin() }}
                >
                  ログイン
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

export default Login;
```

# 51 ユーザーログイン関数を自作する

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
      // console.log(res.data.jwt)
      window.location.href = "/" // 追加
    })
    .catch((err) => {
      console.log(err)
    })
}

// 追加
export const login = async (identifier, password) => {
  await axios.post(`${API_URL}/auth/local`, {
    identifier,
    password
  })
    .then((res) => {
      Cookie.set("token", res.data.jwt, { expires: 7 })
      window.location.href = "/"
    })
    .catch((err) => {
      console.log(err)
    })
}
// ここまで
```

## 52 実際にログインする

+ `frontend/pages/login.js`を編集<br>

```js:login.js
import { useContext, useState } from "react";
import { Button, Col, Container, Form, FormGroup, Input, Label, Row } from "reactstrap";
import AppContext from "../context/AppContext";
import { login } from "../lib/auth";

const Login = () => {
  const appContext = useContext(AppContext)
  const [data, setData] = useState({ identifier: "", password: "" }) // 編集
  console.log(data)

  // 編集
  const handleLogin = () => {
    login(data.identifier, data.password)
      .then((res) => {
        appContext.setUser(res.data.user)
      })
      .catch((err) => console.log(err))
  }
  // ここまで

  const handleChangee = (e) => {
    setData({ ...data, [e.target.name]: e.target.value }) // 追加
  }

  // console.log(data)

  return (
    <Container>
      <Row>
        <Col>
          <div className="paper">
            <div className="header">
              <h2>ログイン</h2>
            </div>
          </div>
          <section className="wrapper">
            <Form>
              <fieldset>
                <FormGroup>
                  <Label>メールアドレス：</Label>
                  <Input
                    type="email"
                    name="identifier"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                    onChange={(e) => handleChangee(e)} // 追加
                  />
                </FormGroup>
                <FormGroup>
                  <Label>パスワード：</Label>
                  <Input
                    type="password"
                    name="password"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                    onChange={(e) => handleChangee(e)} // 追加
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
                  onClick={() => { handleLogin() }}
                >
                  ログイン
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

export default Login;
```

## 53 Promiseオブジェクトを使って修正する

+ `frontend/lib/auth.js`を編集<br>

```js:auth.js
import axios from "axios"
import Cookie from "js-cookie"

const API_URL = process.env.NEXT_PUBLIC_API_URL || "http;//localhost:1337";

// 新しいユーザーを登録
// 編集
export const registerUser = (username, email, password) => {
  return new Promise((resolve, reject) => {
    axios.post(`${API_URL}/auth/local/register`, {
      username,
      email,
      password
    })
      .then((res) => {
        Cookie.set("token", res.data.jwt, { expires: 7 })
        resolve(res)
        window.location.href = "/"
      })
      .catch((err) => {
        reject(err)
        console.log(err)
      })
  })
}

export const login = (identifier, password) => {
  return new Promise((resolve, reject) => {
    axios.post(`${API_URL}/auth/local`, {
      identifier,
      password
    })
      .then((res) => {
        Cookie.set("token", res.data.jwt, { expires: 7 })
        resolve(res)
        window.location.href = "/"
      })
      .catch((err) => {
        reject(err)
        console.log(err)
      })
  })
}
// ここまで
```

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
        appContext.setUser(res.data.user) // 編集
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

## 54 クッキーが残っていればそのユーザーで自動的にログインさせる

+ `frontend/pages/_app.js`を編集<br>

```js:_app.js
import React from "react"
import App from "next/app"
import Head from "next/Head"
import Layout from "../components/Layout"
import withData from "../lib/apollo"
import AppContext from "../context/AppContext"
import Cookies from "js-cookie" // 追加

class MyApp extends App {
  // const [state, setState] = useState(null) と同じものをclassコンポーネントでは下記の書き方になる

  state = {
    user: null,
  }

  setUser = (user) => {
    this.setState({ user })
  }

  // 追加
  // 既にユーザーのクッキー情報が残っているかを確認する
  componentDidMount() {
    const token = Cookies.get("token") // tokenの中にjwtが入ってくる

    if (token) {
      fetch(`${process.env.NEXT_PUBLIC_API_URL}/users/me`, {
        headers: {
          Authorization: `Bearer ${token}`,
        }
      })
        .then(async (res) => {
          if (!res.ok) {
            Cookies.remove("token")
            this.setState({ user: null })
            return null
          }
          const user = await res.json()
          this.setUser(user) // ログイン
        })
    }
  }
  // ここまで

  render() {
    const { Component, pageProps } = this.props
    return (
      <AppContext.Provider value={{ user: this.state.user, setUser: this.setUser }}>
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
      </AppContext.Provider>
    )
  }
}

export default withData(MyApp)
```
