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
