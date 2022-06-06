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
