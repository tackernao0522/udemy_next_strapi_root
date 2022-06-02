## 36 料理のデータ構造を定義する

+ `Strapi ダッシュボード　Content-Types Builderをクリック`<br>

+ `Create new collection type`をクリック<br>

+ `Display name`に`dish`を入力して`Continue`を押下<br>

+ `Text`をクリックして`Name`に`name`を入力して`Add another field`を押下<br>

+ `Text`をクリックして`Name`に`Long text`をクリックして`description`と入力して`Add another field`を押下<br>

+ `Media`をクリックして`Name`に`image`を入力して`Single media`を選択して`Add another field`を押下<br>

+ `Number`を`Name`に`price`と入力して`Number format`に`integer(ex: 10)を選択して`Add another field`を押下<br>

+ `Relation`をクリックして`Field name`に`restaurant`と入力して`4番目の多`をクリックして右側を`Restaurant`に変更して`Restaurant has many Dishes`にして`Finish`を押下して`Save`を押下<br>
