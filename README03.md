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