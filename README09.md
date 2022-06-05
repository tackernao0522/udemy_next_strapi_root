## 62 追加した商品をカートへ出力する

+ `frontend/pages/_app.js`を編集<br>

```js:_app.js
import React from "react"
import App from "next/app"
import Head from "next/Head"
import Layout from "../components/Layout"
import withData from "../lib/apollo"
import AppContext from "../context/AppContext"
import Cookies from "js-cookie"

class MyApp extends App {
  // const [state, setState] = useState(null) と同じものをclassコンポーネントでは下記の書き方になる

  state = {
    user: null,
    //商品保有配列と、合計価格を格納したショッピングカートを用意。
    cart: { items: [], total: 0 },
  };

  setUser = (user) => {
    this.setState({ user })
  }

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

  // カートへ商品の追加
  addItem = (item) => {
    let { items } = this.state.cart
    const newItem = items.find((i) => i.id === item.id)
    // console.log(newItem)
    if (!newItem) {
      item.quantity = 1
      // cartに追加する
      this.setState({
        cart: {
          items: [...items, item],
          total: this.state.cart.total + item.price
        }
      },
        () => Cookies.set("cart", this.state.cart.items)
      )
    }
    // 既に同じ商品がカートに入っている時
    else {
      this.setState({
        cart: {
          items: this.state.cart.items.map((item) =>
            item.id === newItem.id
              ? Object.assign({}, item, { quantity: item.quantity + 1 })
              : item
          ),
          total: this.state.cart.total + item.price
        }
      },
        () => Cookies.set("cart", this.state.cart.items)
      )
    }
  }

  render() {
    const { Component, pageProps } = this.props
    return (
      <AppContext.Provider
        value={{
          user: this.state.user,
          cart: this.state.cart, // 追加
          setUser: this.setUser,
          addItem: this.addItem
        }}
      >
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

+ `frontend/components/Cart/index.js`を編集<br>

```js:index.js
import { Badge, Button, Card, CardBody, CardTitle } from "reactstrap";
import Link from "next/link"
import { useContext } from "react"; // 追加
import AppContext from "../../context/AppContext"; // 追加

const Cart = () => {
  const appContext = useContext(AppContext) // 追加

  const { cart } = appContext // 追加
  return (
    <div>
      <Card style={{ padding: "10px 5px" }}>
        <CardTitle
          style={{
            margin: 10,
            textAlign: "center",
            fontweight: 600,
            fontSize: 25
          }}
        >
          注文一覧
        </CardTitle>
        <hr />
        <CardBody style={{ padding: 10 }}>
          <div style={{ marginBottom: 6 }}>
            <small>料理：</small>
          </div>
          <div>
            // 編集
            {cart.items ? cart.items.map((item) => {
              if (item.quantity > 0) {
                return (
                  <div className="items-one" style={{ marginBottom: 15 }} key={item.id}>
                    <span id="item-price">&nbsp; {item.price}円</span>
                    <span id="item-name">&nbsp; {item.name}</span>
                    <div>
                      <Button
                        style={{
                          height: 25,
                          padding: 0,
                          width: 15,
                          marginRight: 5,
                          marginLeft: 10
                        }}
                        color="link"
                        onClick={() => appContext.addItem(item)}
                      >
                        +
                      </Button>
                      <Button
                        style={{
                          height: 25,
                          padding: 0,
                          width: 15,
                          marginRight: 5,
                          marginLeft: 10
                        }}
                        color="link"
                      >
                        -
                      </Button>
                      <span id="item-quantity" style={{ marginLeft: 5 }}>
                        {item.quantity}つ
                      </span>
                    </div>
                  </div>
                )
              }
            }) : null}
            // ここまで
            <div>
              <Badge style={{ width: 200, padding: 10 }} color="light">
                <h5 style={{ fontWeight: 100, color: "gray" }}>合計：</h5>
                <h3>{cart.total}円</h3>
              </Badge>
              <div>
                <Link href="/checkout">
                  <Button style={{ width: "100%" }} color="primary">
                    <a>注文する</a>
                  </Button>
                </Link>
              </div>
            </div>
          </div>
        </CardBody>
      </Card>
      <style jsx>{`
        #item-price {
          font-size: 1.3em;
          color: rgba(97, 97, 97, 1);
        }
        #item-quantity {
          font-size: 0.95em;
          padding-bottom: 4px;
          color: rgba(158, 158, 158, 1);
        }
        #item-name {
          font-size: 1.3em;
          color: rgba(97, 97, 97, 1);
        }
      `}</style>
    </div>
  );
}

export default Cart;
```

## 63 カートから商品を削除するロジックを構築する

+ `frontend/pages/_app.js`を編集<br>

```js:_app.js
import React from "react"
import App from "next/app"
import Head from "next/Head"
import Layout from "../components/Layout"
import withData from "../lib/apollo"
import AppContext from "../context/AppContext"
import Cookies from "js-cookie"

class MyApp extends App {
  // const [state, setState] = useState(null) と同じものをclassコンポーネントでは下記の書き方になる

  state = {
    user: null,
    //商品保有配列と、合計価格を格納したショッピングカートを用意。
    cart: { items: [], total: 0 },
  };

  setUser = (user) => {
    this.setState({ user })
  }

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

  // カートへ商品の追加
  addItem = (item) => {
    let { items } = this.state.cart
    const newItem = items.find((i) => i.id === item.id)
    // console.log(newItem)
    if (!newItem) {
      item.quantity = 1
      // cartに追加する
      this.setState({
        cart: {
          items: [...items, item],
          total: this.state.cart.total + item.price
        }
      },
        () => Cookies.set("cart", this.state.cart.items)
      )
    }
    // 既に同じ商品がカートに入っている時
    else {
      this.setState({
        cart: {
          items: this.state.cart.items.map((item) =>
            item.id === newItem.id
              ? Object.assign({}, item, { quantity: item.quantity + 1 })
              : item
          ),
          total: this.state.cart.total + item.price
        }
      },
        () => Cookies.set("cart", this.state.cart.items)
      )
    }
  }

  // 追加
  // カートから商品を削除
  removeItem = (item) => {
    let { items } = this.state.cart
    const newItem = items.find((i) => i.id === item.id)
    if (newItem.quantity > 1) {
      this.setState({
        cart: {
          items: this.state.cart.items.map((item) =>
            item.id === newItem.id
              ? Object.assign({}, item, { quantity: item.quantity - 1 })
              : item
          ),
          total: this.state.cart.total - item.price
        }
      },
        () => Cookies.set("cart", this.state.cart.items)
      )
    }
    // カートに入っているその商品が１つの場合
    else {

      const items = [...this.state.cart.items]
      // condole.log(items)
      const index = items.findIndex((i) => i.id === newItem.id)

      items.splice(index, 1)

      this.setState({
        cart: {
          items: items,
          total: this.state.cart.total - item.price
        }
      },
        () => Cookies.set("cart", this.state.cart.items)
      )
    }
  }
  // ここまで

  render() {
    const { Component, pageProps } = this.props
    return (
      <AppContext.Provider
        value={{
          user: this.state.user,
          cart: this.state.cart,
          setUser: this.setUser,
          addItem: this.addItem
        }}
      >
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