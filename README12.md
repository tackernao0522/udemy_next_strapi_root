## 80 実際に料理を注文して決済してみる

+ `frontend/components/Checkout/CheckoutForm.js`を編集<br>

```js:CheckoutForm.js
import { CardElement, useElements, useStripe } from "@stripe/react-stripe-js";
import Cookies from "js-cookie";
import { useContext, useState } from "react";
import { FormGroup, Input, Label } from "reactstrap";
import AppContext from "../../context/AppContext";
import CardSecion from "./CardSecion";

const CheckoutForm = () => {
  const [data, setData] = useState({
    address: "",
    stripe_id: "",
  })

  const elements = useElements()
  const stripe = useStripe()

  const handleChange = (e) => {
    const updateItem = (data[e.target.name] = e.target.value)
    setData({ ...data, updateItem })
  }

  const appContext = useContext(AppContext)
  // 注文を確定させる関数
  const userToken = Cookies.get("token")
  const submitOrder = async () => {
    const cardEelemtnt = elements.getElement(CardElement)
    const token = await stripe.createToken(cardEelemtnt)

    const response = await fetch(`${process.env.NEXT_PUBLIC_API_URL}/orders`, {
      method: "POST",
      headers: userToken && {
        Authorization: `Bearer ${userToken}`,
      },
      body: JSON.stringify({
        amount: Number(appContext.cart.total),
        dishes: appContext.cart.items,
        address: data.address,
        token: token.token.id
      })
    })

    if (response.ok) {
      console.log("注文に成功しました")
    } else {
      console.log("注文に失敗しました")
    }
  }

  return (
    <div className="paper">
      <h5>あなたの情報</h5>
      <hr />
      <FormGroup>
        <div>
          <Label>住所</Label>
          <Input name="address" onChange={(e) => handleChange(e)} />
        </div>
      </FormGroup>

      <CardSecion submitOrder={submitOrder} /> // 編集

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

+ `frontend/components/Checkout/CardSection.js`を編集<br>

```js:CardSection.js
import { CardElement } from "@stripe/react-stripe-js"

const CardSecion = (props) => {
  const { submitOrder } = props
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
              <br />
              <div className="order-button-wrapper">
                <button onClick={submitOrder}>注文を確認</button>
              </div>
            </div>
          </fieldset>
        </div>
      </div>
      <style jsx>
        {`
          .order-button-wrapper {
            display: flex;
            width: 100%;
            align-items: flex-end;
            justify-content: flex-end;
          }
        `}
      </style>
    </div>
  );
}

export default CardSecion;
```

## 81 決済が完了したかどうかを画面に表示させる

+ `frontend/components/Checkout/CheckoutForm.js`を編集<br>

```js:CheckoutForm.js
import { CardElement, useElements, useStripe } from "@stripe/react-stripe-js";
import Cookies from "js-cookie";
import { useContext, useState } from "react";
import { FormGroup, Input, Label } from "reactstrap";
import AppContext from "../../context/AppContext";
import CardSecion from "./CardSecion";

const CheckoutForm = () => {
  const [data, setData] = useState({
    address: "",
    stripe_id: "",
  })
  const [error, setError] = useState("") // 追加
  const [success, setSuccess] = useState("") // 追加

  const elements = useElements()
  const stripe = useStripe()

  const handleChange = (e) => {
    const updateItem = (data[e.target.name] = e.target.value)
    setData({ ...data, updateItem })
  }

  const appContext = useContext(AppContext)
  // 注文を確定させる関数
  const userToken = Cookies.get("token")
  const submitOrder = async () => {
    const cardEelemtnt = elements.getElement(CardElement)
    const token = await stripe.createToken(cardEelemtnt)

    const response = await fetch(`${process.env.NEXT_PUBLIC_API_URL}/orders`, {
      method: "POST",
      headers: userToken && {
        Authorization: `Bearer ${userToken}`,
      },
      body: JSON.stringify({
        amount: Number(appContext.cart.total),
        dishes: appContext.cart.items,
        address: data.address,
        token: token.token.id
      })
    })

    // 編集
    if (response.ok) {
      // console.log("注文に成功しました")
      setSuccess("注文に成功しました")
    } else {
      // console.log("注文に失敗しました")
      setError("注文に失敗しました")
    }
    // ここまで
  }

  return (
    <div className="paper">
      <h5>あなたの情報</h5>
      <hr />
      <FormGroup>
        <div>
          <Label>住所</Label>
          <Input
            name="address"
            onChange={(e) => handleChange(e)}
          /> // 編集
        </div>
      </FormGroup>

      <CardSecion
        submitOrder={submitOrder}
        errorMsg={error}
        successMsg={success}
      />

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

+ `frontend/components/Checkout/CardSeciton.js`を編集<br>

```js:CardSection.js
import { CardElement } from "@stripe/react-stripe-js"

const CardSecion = (props) => {
  const { submitOrder, errorMsg, successMsg } = props // 追加
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
              <br />
              <div className="order-button-wrapper">
                <button onClick={submitOrder}>注文を確認</button>
              </div>
              {errorMsg ? <div>{errorMsg}</div> : null} // 追加
              {successMsg ? <div>{successMsg}</div> : null} // 追加
            </div>
          </fieldset>
        </div>
      </div>
      <style jsx>
        {`
          .order-button-wrapper {
            display: flex;
            width: 100%;
            align-items: flex-end;
            justify-content: flex-end;
          }
        `}
      </style>
    </div>
  );
}

export default CardSecion;
```
