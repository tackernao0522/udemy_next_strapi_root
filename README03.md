## 30 レストランカードをCSSでスタイリング

+ localhost:1337/restaurants/1 にアクセスすると下記のjsonが表示されるので参考にする<br>

```:json
{"id":1,"name":"Italian Restaurant","description":"イタリアンのレストランです。","published_at":"2022-05-30T02:20:22.901Z","created_at":"2022-05-30T02:15:41.838Z","updated_at":"2022-05-30T02:20:22.982Z","image":[{"id":4,"name":"restaurant1.jpg","alternativeText":"","caption":"","width":1920,"height":1280,"formats":{"large":{"ext":".jpg","url":"/uploads/large_restaurant1_a38901cd49.jpg","hash":"large_restaurant1_a38901cd49","mime":"image/jpeg","name":"large_restaurant1.jpg","path":null,"size":101.92,"width":1000,"height":667},"small":{"ext":".jpg","url":"/uploads/small_restaurant1_a38901cd49.jpg","hash":"small_restaurant1_a38901cd49","mime":"image/jpeg","name":"small_restaurant1.jpg","path":null,"size":35.32,"width":500,"height":333},"medium":{"ext":".jpg","url":"/uploads/medium_restaurant1_a38901cd49.jpg","hash":"medium_restaurant1_a38901cd49","mime":"image/jpeg","name":"medium_restaurant1.jpg","path":null,"size":64.03,"width":750,"height":500},"thumbnail":{"ext":".jpg","url":"/uploads/thumbnail_restaurant1_a38901cd49.jpg","hash":"thumbnail_restaurant1_a38901cd49","mime":"image/jpeg","name":"thumbnail_restaurant1.jpg","path":null,"size":11.51,"width":234,"height":156}},"hash":"restaurant1_a38901cd49","ext":".jpg","mime":"image/jpeg","size":325.07,"url":"/uploads/restaurant1_a38901cd49.jpg","previewUrl":null,"provider":"local","provider_metadata":null,"created_at":"2022-05-30T02:11:52.499Z","updated_at":"2022-05-30T02:11:52.535Z"}]}
```

+ 画像のURLはjsonを見ると `"/uploads/large_restaurant1_a38901cd49.jpg"`になる<br>

+ `frontend/components/RestaurantList/index.js`を編集<br>

```js:index.js
import Link from "next/link";
import { Card, CardBody, CardImg, CardTitle, Col, Row } from "reactstrap";

const RestaurantList = () => {
  return (
    <Row>
      <Col xs="6" sm="4">
        <Card style={{ margin: "0 0.5rem 20px 0.5rem" }}>
          <CardImg src="http://localhost:1337/uploads/large_restaurant1_a38901cd49.jpg" top={true} style={{ height: 250 }} />
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
}

export default RestaurantList;
```

## 31 GraphQLからAPIを叩いてデータを取得する

+ localhost:1337/graphql<br>

graphQLの書き方は<br>

```
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
```

```:results
{
  "data": {
    "restaurants": [
      {
        "id": "1",
        "name": "Italian Restaurant",
        "description": "イタリアンのレストランです。",
        "image": [
          {
            "url": "/uploads/restaurant1_a38901cd49.jpg"
          }
        ]
      },
      {
        "id": "2",
        "name": "Japanese Restaurant",
        "description": "日本食専門のレストランです。",
        "image": [
          {
            "url": "/uploads/restaurant2_c66a5f710f.jpg"
          }
        ]
      },
      {
        "id": "3",
        "name": "Party Restaurant",
        "description": "パーティ用のレストランです。",
        "image": [
          {
            "url": "/uploads/restaurant3_e33659c1b4.jpg"
          }
        ]
      }
    ]
  }
}
```

+ `frontend/components/RestaurantsList/index.js`を編集<br>

```js:index.js
import Link from "next/link";
import { Card, CardBody, CardImg, CardTitle, Col, Row } from "reactstrap";
import { gql } from "apollo-boost" // 追加
import { useQuery } from "@apollo/react-hooks" // 追加

// 追加
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
`
// ここまで

const RestaurantList = () => {
  // 追加
  const { loading, error, data } = useQuery(query)
  console.log(data)
  // ここまで
  return (
    <Row>
      <Col xs="6" sm="4">
        <Card style={{ margin: "0 0.5rem 20px 0.5rem" }}>
          <CardImg src="http://localhost:1337/uploads/large_restaurant1_a38901cd49.jpg" top={true} style={{ height: 250 }} />
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
}

export default RestaurantList;
```

+ localhost:3000にアクセスしてコンソールログを確認<br>

```:console
{restaurants: Array(3)}
restaurants: Array(3)
0:
description: "イタリアンのレストランです。"
id: "1"
image: Array(1)
0: {url: '/uploads/restaurant1_a38901cd49.jpg', __typename: 'UploadFile'}
length: 1
[[Prototype]]: Array(0)
name: "Italian Restaurant"
__typename: "Restaurant"
[[Prototype]]: Object
1: {id: '2', name: 'Japanese Restaurant', description: '日本食専門のレストランです。', image: Array(1), __typename: 'Restaurant'}
2: {id: '3', name: 'Party Restaurant', description: 'パーティ用のレストランです。', image: Array(1), __typename: 'Restaurant'}
length: 3
```


## 32 APIから取得したデータでHTMLを書き換える

+ `frontend/components/RestaurantsList/index.js`を編集<br>

```js:index.js
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

const RestaurantList = () => {
  const { loading, error, data } = useQuery(query)

  if (data.restaurants && data.restaurants.length) {
    return (
      <Row>
        {data.restaurants.map((res) => (
          <Col xs="6" sm="4">
            <Card style={{ margin: "0 0.5rem 20px 0.5rem" }}>
              <CardImg src={`${process.env.NEXT_PUBLIC_API_URL}${res.image[0].url}`} top={true} style={{ height: 250 }} />
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

export default RestaurantList;
```

## 33 検索欄に打ち込む値を格納する機能を追加する

+ `frontend/pages/index.js`を編集<br>

```js:index.js
import { useState } from "react";
import { Col, Input, InputGroup, InputGroupText, Row } from "reactstrap"
import RestaurantList from "../components/RestaurantsList";

const index = () => {
  const [query, setQuery] = useState('')

  return (
    <div className="container-fluid">
      <Row>
        <Col>
          <div className="search">
            <InputGroup>
              <InputGroupText>探す</InputGroupText>
              <Input
                placeholder="レストラン名を入力してください"
                // 追加
                onChange={(e) => setQuery(e.target.value.toLocaleLowerCase())}
              />
            </InputGroup>
          </div>
          // 編集
          <RestaurantList search={query} />
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

## 34 検索バーにおけるフィルタリング機能を実装する

+ `frontend/components/RestaurantsList/index.js`を編集<br>

```js:index.js
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

// 編集
const RestaurantList = (props) => {
  const { loading, error, data } = useQuery(query)
  const { search } = props // 追加

  // 追加
  if (data) {
    const searchQuery = data.restaurants.filter((restaurant) =>
      restaurant.name.toLowerCase().includes(search)
    )
    return (
      <Row>
        // 編集
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
                  href={`/restaurants/${res.id}`}
                  as={`/restaurants?id=${res.id}`}
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

export default RestaurantList;
```

## 35 データ取得時の失敗、ローディング時に出力する内容を追加する

+ `frontend/components/RestaurantsList/index.js`を編集<br>

```js:index.js
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

const RestaurantList = (props) => {
  const { loading, error, data } = useQuery(query)
  const { search } = props

  // 追加
  if (error) return "レストランの読み込みに失敗しました。"

  if (loading) return <h1>読み込み中・・・</h1>
  // ここまで

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
                  href={`/restaurants/${res.id}`}
                  as={`/restaurants?id=${res.id}`}
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

export default RestaurantList;
```