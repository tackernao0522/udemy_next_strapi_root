# セクション 5: Next.js でショッピングサイトを構築(ユーザー登録・ログイン編)

## 42 ユーザーアカウント登録に必要なライブラリ群をインストールする

参考: https://docs-v3.strapi.io/developer-docs/latest/development/plugins/users-permissions.html#registration <br>

+ `root $ docker compose run --rm forntend yarn add axios@0.19.2 js-cookie@2.2.1 isomorphic-fetch@2.2.1` を実行<br>

## 43 アカウント登録用のページを作成する

+ `root $ touch frontend/pages/register.js`を実行<br>

(nafe) <br>

+ `frontend/pages/register.js`を編集<br>

```js:register
import { Button, Col, Container, Form, FormGroup, Input, Label, Row } from "reactstrap";

const register = () => {
  return (
    <Container>
      <Row>
        <Col>
          <div className="paper">
            <div className="header">
              <h2>ユーザー登録</h2>
            </div>
          </div>
          <section className="wrapper">
            <Form>
              <fieldset>
                <FormGroup>
                  <Label>ユーザー名：</Label>
                  <Input
                    type="text"
                    name="username"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <FormGroup>
                  <Label>メールアドレス：</Label>
                  <Input
                    type="email"
                    name="email"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <FormGroup>
                  <Label>パスワード：</Label>
                  <Input
                    type="password"
                    name="password"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <span>
                  <a href="">
                    <samall>パスワードをお忘れですか？</samall>
                  </a>
                </span>
                <Button style={{ float: "right", width: 120 }} color="primary">
                  登録
                </Button>
              </fieldset>
            </Form>
          </section>
        </Col>
      </Row>
    </Container>
  );
}

export default register;
```

## 44 アカウント登録ページをCSSでスタイリングする

+ `frontend/pages/register.js`を編集<br>

```js:register.js
import { Button, Col, Container, Form, FormGroup, Input, Label, Row } from "reactstrap";

const register = () => {
  return (
    <Container>
      <Row>
        <Col>
          <div className="paper">
            <div className="header">
              <h2>ユーザー登録</h2>
            </div>
          </div>
          <section className="wrapper">
            <Form>
              <fieldset>
                <FormGroup>
                  <Label>ユーザー名：</Label>
                  <Input
                    type="text"
                    name="username"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <FormGroup>
                  <Label>メールアドレス：</Label>
                  <Input
                    type="email"
                    name="email"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <FormGroup>
                  <Label>パスワード：</Label>
                  <Input
                    type="password"
                    name="password"
                    style={{ height: 50, fontsize: "1.2 rem" }}
                  />
                </FormGroup>
                <span>
                  <a href="">
                    <samall>パスワードをお忘れですか？</samall>
                  </a>
                </span>
                <Button style={{ float: "right", width: 120 }} color="primary">
                  登録
                </Button>
              </fieldset>
            </Form>
          </section>
        </Col>
      </Row>
      // 追加
      <style jsx>
        {`
          .paper {
            text-align: center;
            margin-top: 50px;
          }
          .header {
            width: 100%;
            margin-bottom: 30px;
          }
          .wrapper {
            padding: 10px 30px 20px 30px;
          }
        `}
      </style>
      // ここまで
    </Container>
  );
}

export default register;
```