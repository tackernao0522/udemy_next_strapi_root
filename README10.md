# セクション 7: Next.js でショッピングサイトを構築(決済システム編)

## 66 注文用のコレクションを新しく追加する

+ `Strapi`ダッシュボードの`Content-Types Builder`を選択する<br>

+ `Create new colleciton type`をクリック<br>

+ `Display name`に`order`を入力して`続ける`をクリック<br>

+ `Text`をクリックして、`Name`に`address`と入力して`Short text`のまま`Add another field`をクリック<br>

+ `JSON`をクリックして、`Name`に`dishes`を入力して`Add another field`をクリック<br>

+ `Number`をクリックして、`Name`に`amount`を入力して、`数値形式`を`big integer(ex: 123456789)`を選択して`Finish`をクリック<br>

+ `保存`をクリック<br>

+ `設定`をクリック<br>

+ `USERS & PERMISSIONS PLUGIN`の`ロールと権限`をクリック<br>

+ `Authenticated`をクリック<br>

+ `ORDER`の`create`にチェックを入れる<br>

+ `DISH`の`find`と`findone`にチェックを入れる<br>

+ `保存`をクリック<br>

## 67 決済システム用にStripeに登録する

参考: https://stripe.com/docs/api <br>

## 68 バックエンド側で注文用ロジックを構築する

+ `backend/app/api/order/controllers/order.js`を編集<br>

```js:order.js
'use strict';

/**
 * Read the documentation (https://strapi.io/documentation/developer-docs/latest/development/backend-customization.html#core-controllers)
 * to customize this controller
 */

module.exports = {
  // 注文を作成する
  create: async (ctx) => {
    const { address, amount, dishes, token } = JSON.parse(ctx.request.body)
    const charge =
  }
};
```

## 69 Stripeドキュメントを見ながら料金支払いロジックを構築する

参考: https://stripe.com/docs/api/charges/create <br>

+ `backend/app/api/order/controllers/order.js`を編集<br>

```js:order.js
'use strict';

/**
 * Read the documentation (https://strapi.io/documentation/developer-docs/latest/development/backend-customization.html#core-controllers)
 * to customize this controller
 */
const stripe = require('stripe')('sk_test_51L7W89JIrxSQKjakzpMfm92vcUI6wue3TUZkIoEAT9Vw4hmvCAmoqkdYsUVa5JlDwPKTbXsFNROYvdhcxDc7FK5x007NdbmJF5');

module.exports = {
  // 注文を作成する
  create: async (ctx) => {
    const { address, amount, dishes, token } = JSON.parse(ctx.request.body)
    const charge = await stripe.charges.create({
      amount: amount,
      currency: 'jpy',
      source: token,
      description: `Order ${new Date()} by ${ctx.state.user._id}`,
    });

    const order = await strapi.services.order.create({
      user: ctx.state.user._id,
      charge_id: charge.id,
      amount: amount,
      address,
      dishes
    })

    return order
  }
};
```

## 70 バックエンドにStripeライブラリをインストールする

+ `root $ docker compose run --rm backend yarn add stripe -D`を実行<br>

## 72 チェックアウトページを作成する

+ `root $ mkdir frontend/components/Checkout && touch $_/CheckoutForm.js`を実行<br>

`nafe`<br>

+ `frontend/components/Checkout/CheckoutForm.js`を編集<br>

```js:CheckoutForm.js
const CheckoutForm = () => {
  return (
    <div>
      Enter
    </div>
  );
}

export default CheckoutForm;
```

+ `root $ touch frontend/pages/checkout.js`を実行<br>

+ `frontend/pages/checkout.js`を編集<br>

`nafe`<br>

```js:checkout.js
import { Col, Row } from "reactstrap";
import Cart from "../components/Cart/index"
import CheckoutForm from "../components/Checkout/CheckoutForm";

const checkout = () => {
  return (
    <Row>
      <Col>
        <h1>チェックアウト</h1>
        <Cart />
      </Col>
      <Col>
        <CheckoutForm />
      </Col>
    </Row>
  );
}

export default checkout;
```

## 73 チェックアウトページをCSSでスタイリングする

+ `frontend/pages/checkout.js`を編集<br>

```js:checkout.js
import { Col, Row } from "reactstrap";
import Cart from "../components/Cart/index"
import CheckoutForm from "../components/Checkout/CheckoutForm";

const checkout = () => {
  return (
    <Row>
      <Col style={{ paddingRight: 0 }} sm={{ size: 3, order: 1, offset: 2 }}> // 編集
        <h1 style={{ margin: 20, fontSize: 20, textAlign: "center" }}> // 編集
          チェックアウト
        </h1>
        <Cart />
      </Col>
      <Col style={{ paddingLeft: 5 }} sm={{ size: 6, order: 2 }}> // 編集
        <CheckoutForm />
      </Col>
    </Row>
  );
}

export default checkout;
```

## 74 住所やカード情報を打ち込むコンポーネントを作成する

+ `root $ touch frontend/components/Checkout/CardSecion.js`を実行<br>

+ `frontend/components/Checkout/CardSection.js`を編集<br>

```js:CardSection.js
const CardSecion = () => {
  return (
    <div>
      Enter
    </div>
  );
}

export default CardSecion;
```

+ `frontend/components/Checkout/CheckoutForm.js`を編集<br>

```js:CheckoutForm.js
import { FormGroup, Input, Label } from "reactstrap";
import CardSecion from "./CardSecion";

const CheckoutForm = () => {
  return (
    <div className="paper">
      <h5>あなたの情報</h5>
      <hr />
      <FormGroup>
        <div>
          <Label>住所</Label>
          <Input name="address" />
        </div>
      </FormGroup>

      <CardSecion />

      <style jsx global>
        {`
          .paper {
            border: 1px solid lightgray;
            box-shadow: 0px 1px 3px 0px rgba(0, 0, 0, 0.2),
              0px 1px 1px 0px rgba(0, 0, 0, 0.14),
              0px 2px 1px -1px rgba(0, 0, 0, 0.12);
            height: 550px;
            padding: 30px;
            background: #fff;
            border-radius: 6px;
            margin-top: 90px;
          }
          .form-half {
            flex: 0.5;
          }
          * {
            box-sizing: border-box;
          }
          body,
          html {
            background-color: #f6f9fc;
            font-size: 18px;
            font-family: Helvetica Neue, Helvetica, Arial, sans-serif;
          }
          h1 {
            color: #32325d;
            font-weight: 400;
            line-height: 50px;
            font-size: 40px;
            margin: 20px 0;
            padding: 0;
          }
          .Checkout {
            margin: 0 auto;
            max-width: 800px;
            box-sizing: border-box;
            padding: 0 5px;
          }
          label {
            color: #6b7c93;
            font-weight: 300;
            letter-spacing: 0.025em;
          }
          button {
            white-space: nowrap;
            border: 0;
            outline: 0;
            display: inline-block;
            height: 40px;
            line-height: 40px;
            padding: 0 14px;
            box-shadow: 0 4px 6px rgba(50, 50, 93, 0.11),
              0 1px 3px rgba(0, 0, 0, 0.08);
            color: #fff;
            border-radius: 4px;
            font-size: 15px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.025em;
            background-color: #6772e5;
            text-decoration: none;
            -webkit-transition: all 150ms ease;
            transition: all 150ms ease;
            margin-top: 10px;
          }
          form {
            margin-bottom: 40px;
            padding-bottom: 40px;
            border-bottom: 3px solid #e6ebf1;
          }
          button:hover {
            color: #fff;
            cursor: pointer;
            background-color: #7795f8;
            transform: translateY(-1px);
            box-shadow: 0 7px 14px rgba(50, 50, 93, 0.1),
              0 3px 6px rgba(0, 0, 0, 0.08);
          }
          input,
          .StripeElement {
            display: block;
            background-color: #f8f9fa !important;
            margin: 10px 0 20px 0;
            max-width: 500px;
            padding: 10px 14px;
            font-size: 1em;
            font-family: "Source Code Pro", monospace;
            box-shadow: rgba(50, 50, 93, 0.14902) 0px 1px 3px,
              rgba(0, 0, 0, 0.0196078) 0px 1px 0px;
            border: 0;
            outline: 0;
            border-radius: 4px;
            background: white;
          }
          input::placeholder {
            color: #aab7c4;
          }
          input:focus,
          .StripeElement--focus {
            box-shadow: rgba(50, 50, 93, 0.109804) 0px 4px 6px,
              rgba(0, 0, 0, 0.0784314) 0px 1px 3px;
            -webkit-transition: all 150ms ease;
            transition: all 150ms ease;
          }
          .StripeElement.IdealBankElement,
          .StripeElement.PaymentRequestButton {
            padding: 0;
          }
        `}
      </style>
    </div>
  );
}

export default CheckoutForm;
```

## 75 カード情報を打ち込むためのCardSectionコンポーネントを作成する

+ `root $ docker compose run --rm frontend yarn add @stripe/react-stripe-js`を実行<br>

+ `frontend/components/Checkout/CardSeciton.js`を編集<br>

```js:CardSection.js
import { CardElement } from "@stripe/react-stripe-js"

const CardSecion = () => {
  return (
    <div>
      <div>
        <label htmlFor="card-element">クレジット/デビットカード</label>

        <div>
          <fieldset>
            <div className="form-row">
              <div id="card-element" style={{ width: "100%" }}>
                <CardElement />
              </div>
            </div>
          </fieldset>
        </div>
      </div>
    </div>
  );
}

export default CardSecion;
```

+ `root $ docker compose run --rm frontend yarn add @stripe/stripe-js`を実行<br>

+ `frontend/pages/checkout.js`を編集<br>

```js:checkout.js
import { Elements } from "@stripe/react-stripe-js"; // 追加
import { loadStripe } from "@stripe/stripe-js" // 追加
import { Col, Row } from "reactstrap";
import Cart from "../components/Cart/index"
import CheckoutForm from "../components/Checkout/CheckoutForm";

const checkout = () => {
  // 追加
  const stripePromise = loadStripe(
    "pk_test_51L7W89JIrxSQKjakE1p3wWfbeAZGsMVSCoaeFGs5GXzvzCooY8Q11VFKeUhIFncBkgF6esJgvwNvY8jgop29Puy800gEkdfyQ2"
  )
  // ここまで
  return (
    <Row>
      <Col style={{ paddingRight: 0 }} sm={{ size: 3, order: 1, offset: 2 }}>
        <h1 style={{ margin: 20, fontSize: 20, textAlign: "center" }}>
          チェックアウト
        </h1>
        <Cart />
      </Col>
      <Col style={{ paddingLeft: 5 }} sm={{ size: 6, order: 2 }}>
        // 追加
        <Elements stripe={stripePromise}>
          <CheckoutForm />
        </Elements>
        // ここまで
      </Col>
    </Row>
  );
}

export default checkout;
```
