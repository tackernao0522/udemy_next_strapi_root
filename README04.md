## 36 料理のデータ構造を定義する

+ `Strapi ダッシュボード　Content-Types Builderをクリック`<br>

+ `Create new collection type`をクリック<br>

+ `Display name`に`dish`を入力して`Continue`を押下<br>

+ `Text`をクリックして`Name`に`name`を入力して`Add another field`を押下<br>

+ `Text`をクリックして`Name`に`Long text`をクリックして`description`と入力して`Add another field`を押下<br>

+ `Media`をクリックして`Name`に`image`を入力して`Single media`を選択して`Add another field`を押下<br>

+ `Number`を`Name`に`price`と入力して`Number format`に`integer(ex: 10)を選択して`Add another field`を押下<br>

+ `Relation`をクリックして`Field name`に`restaurant`と入力して`4番目の多`をクリックして右側を`Restaurant`に変更して`Restaurant has many Dishes`にして`Finish`を押下して`Save`を押下<br>

## 37 レストラン個別に料理データを追加する

+ Strapi ダッシュボードの `COLLECTION TYPES`の`Dishes`をクリック<br>

1. `Add New dishes`をクリック`<br>

2. `Image`に`Food画像`を4枚入れる<br>

3. `Finish`をクリック<br>

4. `Image`に`サラダ画像`を選択<br>

5. `Name`に`サラダ`を入力<br>

6. `Description`に`さっぱりとしたサラダです。`と入力<br>

7. `Price`に`250'を入力<br>

8. `Restaurant`を`Italian Restaurant`を選択<br>

9. `Save`をクリック<br>

10. `Publish`をクリック<br>

+ 残りの3枚Food画像に合ったDataを1〜10を繰り返して登録する<br>

+ `Settings`をクリック<br>

+ `USERS & PERMISSIONS PLUGIN`の`Roles`をクリック<br>

+ `Public`をクリック<br>

+ `DISH`の`find`と`findOne`にチェックを入れて`Save`をクリック<br>

+ localhost:1337/dishes/1 <br>

```:json
{
  "id": 1,
  "name": "サラダ",
  "description": "さっぱりとしたサラダです。",
  "prince": 250,
  "restaurant": {
    "id": 1,
    "name": "Italian Restaurant",
    "description": "イタリアンのレストランです。",
    "published_at": "2022-06-01T17:38:46.169Z",
    "created_at": "2022-06-01T17:38:42.120Z",
    "updated_at": "2022-06-01T17:38:46.229Z",
    "image": [{
      "id": 3,
      "name": "restaurant1.jpg",
      "alternativeText": "",
      "caption": "",
      "width": 1920,
      "height": 1280,
      "formats": {
        "large": {
          "ext": ".jpg",
          "url": "/uploads/large_restaurant1_7db92ca083.jpg",
          "hash": "large_restaurant1_7db92ca083",
          "mime": "image/jpeg",
          "name": "large_restaurant1.jpg",
          "path": null,
          "size": 101.92,
          "width": 1000,
          "height": 667
        },
        "small": {
          "ext": ".jpg",
          "url": "/uploads/small_restaurant1_7db92ca083.jpg",
          "hash": "small_restaurant1_7db92ca083",
          "mime": "image/jpeg",
          "name": "small_restaurant1.jpg",
          "path": null,
          "size": 35.32,
          "width": 500,
          "height": 333
        },
        "medium": {
          "ext": ".jpg",
          "url": "/uploads/medium_restaurant1_7db92ca083.jpg",
          "hash": "medium_restaurant1_7db92ca083",
          "mime": "image/jpeg",
          "name": "medium_restaurant1.jpg",
          "path": null,
          "size": 64.03,
          "width": 750,
          "height": 500
        },
        "thumbnail": {
          "ext": ".jpg",
          "url": "/uploads/thumbnail_restaurant1_7db92ca083.jpg",
          "hash": "thumbnail_restaurant1_7db92ca083",
          "mime": "image/jpeg",
          "name": "thumbnail_restaurant1.jpg",
          "path": null,
          "size": 11.51,
          "width": 234,
          "height": 156
        }
      },
      "hash": "restaurant1_7db92ca083",
      "ext": ".jpg",
      "mime": "image/jpeg",
      "size": 325.07,
      "url": "/uploads/restaurant1_7db92ca083.jpg",
      "previewUrl": null,
      "provider": "local",
      "provider_metadata": null,
      "created_at": "2022-06-01T17:38:30.760Z",
      "updated_at": "2022-06-01T17:38:31.068Z"
    }]
  },
  "published_at": "2022-06-02T06:23:14.600Z",
  "created_at": "2022-06-02T06:22:53.998Z",
  "updated_at": "2022-06-02T06:23:14.731Z",
  "image": {
    "id": 5,
    "name": "food01.jpg",
    "alternativeText": "",
    "caption": "",
    "width": 1280,
    "height": 814,
    "formats": {
      "large": {
        "ext": ".jpg",
        "url": "/uploads/large_food01_ae78425646.jpg",
        "hash": "large_food01_ae78425646",
        "mime": "image/jpeg",
        "name": "large_food01.jpg",
        "path": null,
        "size": 120.71,
        "width": 1000,
        "height": 636
      },
      "small": {
        "ext": ".jpg",
        "url": "/uploads/small_food01_ae78425646.jpg",
        "hash": "small_food01_ae78425646",
        "mime": "image/jpeg",
        "name": "small_food01.jpg",
        "path": null,
        "size": 36.12,
        "width": 500,
        "height": 318
      },
      "medium": {
        "ext": ".jpg",
        "url": "/uploads/medium_food01_ae78425646.jpg",
        "hash": "medium_food01_ae78425646",
        "mime": "image/jpeg",
        "name": "medium_food01.jpg",
        "path": null,
        "size": 72.33,
        "width": 750,
        "height": 477
      },
      "thumbnail": {
        "ext": ".jpg",
        "url": "/uploads/thumbnail_food01_ae78425646.jpg",
        "hash": "thumbnail_food01_ae78425646",
        "mime": "image/jpeg",
        "name": "thumbnail_food01.jpg",
        "path": null,
        "size": 11.28,
        "width": 245,
        "height": 156
      }
    },
    "hash": "food01_ae78425646",
    "ext": ".jpg",
    "mime": "image/jpeg",
    "size": 196.85,
    "url": "/uploads/food01_ae78425646.jpg",
    "previewUrl": null,
    "provider": "local",
    "provider_metadata": null,
    "created_at": "2022-06-02T06:15:24.460Z",
    "updated_at": "2022-06-02T06:15:24.668Z"
  }
}
```

## 38 レストラン固有のページを作成する

+ `root $ touch frontend/pages/restaurants.js`を実行<br>

+ `frontend/pages/restaurants.js`を編集<br>

```js:restaurants.js
import Link from "next/link";
import { Card, CardBody, CardImg, CardTitle, Col, Row } from "reactstrap";
import { gql } from "apollo-boost"
import { useQuery } from "@apollo/react-hooks"

const query = gql`
  {
    restaurants {
      id
      name
      description
      image {
        url
      }
    }
  }
`;

const Restaurants = (props) => {
  const { loading, error, data } = useQuery(query)
  const { search } = props

  if (error) return "レストランの読み込みに失敗しました。"

  if (loading) return <h1>読み込み中・・・</h1>

  if (data) {
    const searchQuery = data.restaurants.filter((restaurant) =>
      restaurant.name.toLowerCase().includes(search)
    )
    return (
      <Row>
        {searchQuery.map((res) => (
          <Col xs="6" sm="4" key={res.id}>
            <Card style={{ margin: "0 0.5rem 20px 0.5rem" }}>
              <CardImg
                src={`${process.env.NEXT_PUBLIC_API_URL}${res.image[0].url}`}
                top={true}
                style={{ height: 250 }}
              />
              <CardBody>
                <CardTitle>{res.name}</CardTitle>
                <CardTitle>{res.description}</CardTitle>
              </CardBody>
              <div className="card-footer">
                <Link
                  as={`/restaurants/${res.id}`}
                  href={`/restaurants?id=${res.id}`}
                >
                  <a className="btn btn-primary">もっと見る</a>
                </Link>
              </div>
            </Card>
          </Col>
        ))}

        <style jsx>
          {`
            a {
              color: white;
            }
            a;link {
              text-decoration: none;
              color: white;
            }
            a:hover {
              color: white;
            }
            .card-columns {
              column-count: 3;
            }
          `}
        </style>
      </Row>
    );
  } else {
    return <h1>レストランが見つかりませんでした。</h1>
  }
}

export default Restaurants;
```

## 39 レストランIDを使って個別にAPIを叩く

+ `frontend/pages/restaurants.js`を編集<br>

```js:restaurants.js
import Link from "next/link";
import { Card, CardBody, CardImg, CardTitle, Col, Row } from "reactstrap";
import { gql } from "apollo-boost"
import { useQuery } from "@apollo/react-hooks"
import { useRouter } from "next/router" // 追加

// 編集
const GET_RESTAURANT_DISHES = gql`
    {
      query ($id: ID!) {
        restaurant(id: $id) {
          id
          name
          dishes {
            id
            description
            price
            image {
              url
            }
          }
        }
      }
    }
`;
// ここまで

const Restaurants = (props) => {
  // 編集
  const router = useRouter()
  const { loading, error, data } = useQuery(GET_RESTAURANT_DISHES, {
    variables: { id: router.query.id },
  })
  // ここまで

  const { search } = props

  if (error) return "レストランの読み込みに失敗しました。"

  if (loading) return <h1>読み込み中・・・</h1>

  if (data) {
    const searchQuery = data.restaurants.filter((restaurant) =>
      restaurant.name.toLowerCase().includes(search)
    )
    return (
      <Row>
        {searchQuery.map((res) => (
          <Col xs="6" sm="4" key={res.id}>
            <Card style={{ margin: "0 0.5rem 20px 0.5rem" }}>
              <CardImg
                src={`${process.env.NEXT_PUBLIC_API_URL}${res.image[0].url}`}
                top={true}
                style={{ height: 250 }}
              />
              <CardBody>
                <CardTitle>{res.name}</CardTitle>
                <CardTitle>{res.description}</CardTitle>
              </CardBody>
              <div className="card-footer">
                <Link
                  as={`/restaurants/${res.id}`}
                  href={`/restaurants?id=${res.id}`}
                >
                  <a className="btn btn-primary">もっと見る</a>
                </Link>
              </div>
            </Card>
          </Col>
        ))}

        <style jsx>
          {`
            a {
              color: white;
            }
            a;link {
              text-decoration: none;
              color: white;
            }
            a:hover {
              color: white;
            }
            .card-columns {
              column-count: 3;
            }
          `}
        </style>
      </Row>
    );
  } else {
    return <h1>レストランが見つかりませんでした。</h1>
  }
}

export default Restaurants;
```

## 40 料理データをページに出力して表示させる

+ `frontend/pages/restaurants.js`を編集<br>

```js:restaurants.js
import Link from "next/link";
import { Card, CardBody, CardImg, CardTitle, Col, Row } from "reactstrap";
import { gql } from "apollo-boost"
import { useQuery } from "@apollo/react-hooks"
import { useRouter } from "next/router" // 追加

// 編集
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

// 編集
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
      <Row>
        {restaurant.dishes.map((res) => (
          <Col xs="6" sm="4" key={res.id}>
            <Card style={{ margin: "0 0.5rem 20px 0.5rem" }}>
              <CardImg
                src={`${process.env.NEXT_PUBLIC_API_URL}${res.image.url}`} // 編集
                top={true}
                style={{ height: 250 }}
              />
              <CardBody>
                <CardTitle>{res.name}</CardTitle>
                <CardTitle>{res.description}</CardTitle>
              </CardBody>
              <div className="card-footer">
                <Link
                  as={`/restaurants/${res.id}`}
                  href={`/restaurants?id=${res.id}`}
                >
                  <a className="btn btn-primary">もっと見る</a>
                </Link>
              </div>
            </Card>
          </Col>
        ))}

        <style jsx>
          {`
            a {
              color: white;
            }
            a;link {
              text-decoration: none;
              color: white;
            }
            a:hover {
              color: white;
            }
            .card-columns {
              column-count: 3;
            }
          `}
        </style>
      </Row>
    );
  } else {
    return <h1>レストランが見つかりませんでした。</h1>
  }
}

export default Restaurants; // 編集
```

## 41 レストラン固有ページのレイアウトを修正

+ `frontend/pages/restaurants.js`を編集<br>

```js:restaurants.js
import { Button, Card, CardBody, CardImg, CardTitle, Col, Row } from "reactstrap"; // 編集
import { gql } from "apollo-boost"
import { useQuery } from "@apollo/react-hooks"
import { useRouter } from "next/router"

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
      <> // 追加
        <h1>{restaurant.name}</h1> // 追加
        <Row>
          {restaurant.dishes.map((res) => (
            <Col xs="6" sm="4" key={res.id} style={{ padding: 0 }}> // 編集
              <Card style={{ margin: "0 10px" }}> // 編集
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
                  // 編集
                  <Button outline color="primary">
                    + カートに入れる
                  </Button>
                  // ここまで
                </div>
              </Card>
            </Col>
          ))}
        </Row>
      </>
    );
  } else {
    return <h1>レストランが見つかりませんでした。</h1>
  }
}

export default Restaurants;
```