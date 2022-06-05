## postgres への入り方

- `$ docker compose run --rm postgresql sh`を実行<br>

- `/ # psql -U strapi`を実行<br>

- `\d`を実行するとテーブル一覧が確認できる<br>

# セクション 6: Next.js でショッピングサイトを構築(ショッピングカート編)

## 55 ショッピングカート用コンポーネントを作成する (その 1)

+ `root $ mkdir frontend/components/Cart && touch $_/index.js`<br>

+ `frontend/components/Cart/index.js`を編集<br>

+ `nafe`<br>

```js:index.js
const Cart = () => {
  return (
    <div>
      Enter
    </div>
  );
}

export default Cart;
```

+ `frontend/pages/restaurants.js`を編集<br>

```js:restaurants.js
import { Button, Card, CardBody, CardImg, CardTitle, Col, Row } from "reactstrap";
import { gql } from "apollo-boost"
import { useQuery } from "@apollo/react-hooks"
import { useRouter } from "next/router"
import Cart from "../components/Cart";

const GET_RESTAURANT_DISHES = gql`
  query ($id: ID!) {
    restaurant(id: $id) {
      id
      name
      dishes {
        id
        name
        description
        price
        image {
          url
        }
      }
    }
  }
`

const Restaurants = () => {
  const router = useRouter()
  const { loading, error, data } = useQuery(GET_RESTAURANT_DISHES, {
    variables: { id: router.query.id }
  })
  // console.log(data)

  if (error) return "レストランの読み込みに失敗しました。"

  if (loading) return <h1>読み込み中・・・</h1>

  if (data) {
    const { restaurant } = data
    return (
      <>
        <h1>{restaurant.name}</h1>
        <Row>
          {restaurant.dishes.map((res) => (
            <Col xs="6" sm="4" key={res.id} style={{ padding: 0 }}>
              <Card style={{ margin: "0 10px" }}>
                <CardImg
                  src={`${process.env.NEXT_PUBLIC_API_URL}${res.image.url}`}
                  top={true}
                  style={{ height: 250 }}
                />
                <CardBody>
                  <CardTitle>{res.name}</CardTitle>
                  <CardTitle>{res.description}</CardTitle>
                </CardBody>
                <div className="card-footer">
                  <Button outline color="primary">
                    + カートに入れる
                  </Button>
                </div>
              </Card>
            </Col>
          ))}
          // 追加
          <Col xs="3" style={{ padding: 0 }}>
            <div>
              <Cart />
            </div>
          </Col>
          // ここまで
        </Row>
      </>
    );
  } else {
    return <h1>レストランが見つかりませんでした。</h1>
  }
}

export default Restaurants;
```

+ `frontend/components/Cart/index.js`を編集<br>

```js:index.js
import { Card, CardBody, CardTitle } from "reactstrap";
import Link from "next/link"

const Cart = () => {
  return (
    <div>
      <Card>
        <CardTitle>
          注文一覧
        </CardTitle>
        <hr />
        <CardBody>
          <div>
            <small>料理：</small>
          </div>
          <div>
            <div className="items-one">
              <span id="item-price">&nbsp; 200円</span>
              <span id="item-name">&nbsp; サラダ</span>
              <div>
                <Button>
                  +
                </Button>
                <Button>
                  -
                </Button>
                <span id="item-quantity">１つ</span>
              </div>
            </div>
          </div>
        </CardBody>
      </Card>
    </div>
  );
}

export default Cart;
```

## 56 ショッピングカート用コンポーネントを作成する (その2)

+ `frontend/components/Cart/index.js`を編集<br>

```js:index.js
import { Badge, Button, Card, CardBody, CardTitle } from "reactstrap"; // 編集
import Link from "next/link" // 追加

const Cart = () => {
  return (
    <div>
      <Card>
        <CardTitle>
          注文一覧
        </CardTitle>
        <hr />
        <CardBody>
          <div>
            <small>料理：</small>
          </div>
          <div>
            <div className="items-one">
              <span id="item-price">&nbsp; 200円</span>
              <span id="item-name">&nbsp; サラダ</span>
              <div>
                <Button>
                  +
                </Button>
                <Button>
                  -
                </Button>
                <span id="item-quantity">１つ</span>
              </div>
            </div>
            // 追加
            <div>
              <Badge>
                <h5>合計：</h5>
                <h3>1200円</h3>
              </Badge>
              <div>
                <Link href="/checkout">
                  <Button>
                    <a>注文する</a>
                  </Button>
                </Link>
              </div>
            </div>
            // ここまで
          </div>
        </CardBody>
      </Card>
    </div>
  );
}

export default Cart;
```

## 57 ショッピングカートをCSSでスタイリングする

+ `frontend/components/Cart/index.js`を編集<br>

```js:index.js
import { Badge, Button, Card, CardBody, CardTitle } from "reactstrap";
import Link from "next/link"

const Cart = () => {
  return (
    <div>
      // 編集
      <Card style={{ padding: "10px 5px" }}>
        <CardTitle
          style={{
            margin: 10,
            textAlign: "center",
            fontweight: 600,
            fontSize: 25
          }}
        >
        // ここまで
          注文一覧
        </CardTitle>
        <hr />
        <CardBody style={{ padding: 10 }}> // 編集
          <div style={{ marginBottom: 6 }}> // 編集
            <small>料理：</small>
          </div>
          <div>
            <div className="items-one" style={{ marginBottom: 15 }}> // 編集
              <span id="item-price">&nbsp; 200円</span>
              <span id="item-name">&nbsp; サラダ</span>
              <div>
                // 編集
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
                 // ここまで
                  -
                </Button>
                <span id="item-quantity" style={{ marginLeft: 5 }}> // 編集
                  １つ
                </span>
              </div>
            </div>
            <div>
              <Badge style={{ width: 200, padding: 10 }} color="light"> // 編集
                <h5 style={{ fontWeight: 100, color: "gray" }}>合計：</h5> // 編集
                <h3>1200円</h3>
              </Badge>
              <div>
                <Link href="/checkout">
                  <Button style={{ width: "100%" }} color="primary"> // 編集
                    <a>注文する</a>
                  </Button>
                </Link>
              </div>
            </div>
          </div>
        </CardBody>
      </Card>
      // 追加
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
      // ここまで
    </div>
  );
}

export default Cart;
```

## 58 カートに商品を追加するロジックを構築する

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
    // 追加
    cart: {
      items: [],
      total: 0,
    }
    // ここまで
  }

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

  // 追加
  // カートへ商品の追加
  addItem = (item) => {
    let { items } = this.state.cart
    const newItem = items.find((i) => i.id === item.id)
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

## 59 既にカートに同じ商品が入っている場合のロジックを構築する

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
    cart: {
      items: [],
      total: 0,
    }
  }

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
    // 追加
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
    // ここまで
  }

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
