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