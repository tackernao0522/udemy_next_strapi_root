60 商品フィールドに数量のフィールドを追加する

+ `Strapi ダッシュボード コレクションタイプの`Dishes`をクリック<br>

+ `ペンボタンをクリック`<br>

+ `Edit the field`をクリック<br>

+ `Add another field`をクリック<br>

+ `Number`をクリック<br>

+ `Name`に`quantity`を入力、`数値形式`に`integer(ex: 10)`を選択する<br>

+ `Finish`をクリック<br>

+ `保存`をクリック<br>

+ コレクションタイプの`Dishes`をクリックして各商品のペンボタンをクリックして`quantity`に`0`を入れる<br>

## 61 実際にカートに商品を追加する

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
          setUser: this.setUser,
          addItem: this.addItem // 追加
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

+ `frontend/pages/restaurants.js`を編集<br>

```js:restaurants.js
import { Button, Card, CardBody, CardImg, CardTitle, Col, Row } from "reactstrap";
import { gql } from "apollo-boost"
import { useQuery } from "@apollo/react-hooks"
import { useRouter } from "next/router"
import Cart from "../components/Cart";
import { useContext } from "react";
import AppContext from "../context/AppContext";

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
  const appContext = useContext(AppContext)
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
          {restaurant.dishes.map((dish) => (
            <Col xs="6" sm="4" key={dish.id} style={{ padding: 0 }}>
              <Card style={{ margin: "0 10px" }}>
                <CardImg
                  src={`${process.env.NEXT_PUBLIC_API_URL}${dish.image.url}`}
                  top={true}
                  style={{ height: 250 }}
                />
                <CardBody>
                  <CardTitle>{dish.name}</CardTitle>
                  <CardTitle>{dish.description}</CardTitle>
                </CardBody>
                <div className="card-footer">
                  <Button
                    outline
                    color="primary"
                    onClick={() => appContext.addItem(dish)}
                  >
                    + カートに入れる
                  </Button>
                </div>
              </Card>
            </Col>
          ))}
          <Col xs="3" style={{ padding: 0 }}>
            <div>
              <Cart />
            </div>
          </Col>
        </Row>
      </>
    );
  } else {
    return <h1>レストランが見つかりませんでした。</h1>
  }
}

export default Restaurants;
```