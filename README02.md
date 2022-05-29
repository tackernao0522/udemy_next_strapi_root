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