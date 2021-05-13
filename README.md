<h1> MS Tech Camp #7 </br>
Custom Vision Serviceで画像認識AIを作ろう！</h1>

[MS Tech Camp #7](https://mspjp.connpass.com/event/) で使用するハンズオン資料です。

Custom Vision Service で画像認識モデルを構築し、 APIを用いて送信した画像の認識結果を受け取ります。

## 1. Custom Vision Service プロジェクトを作成する

1-1 ブラウザで [Custom Vision Service ポータル](https://www.customvision.ai/) を開きます。 その後、[サインイン] を選択します。

1-2 サインインを求められたら、Microsoft アカウントの資格証明を使用してサインインします。 このアプリにユーザー情報へのアクセスを許可するように求められたら、[はい] をクリックし、プロンプトが表示されたらサービス使用条件に同意します。

1-3 [新しいプロジェクト] をクリックして、新しいプロジェクトを作成します。

![](images/1-portal-click-new-project.png)

1-4 [新しいプロジェクトの作成] ダイアログで、プロジェクトに "Artworks" という名前を付けます。

1-5 このプロジェクトに使用する リソース グループ を選択します。 リソース グループをまだ用意していない場合は、[新規作成] を選択して新しいリソース グループを作成します。

1-6 必要事項を記入します。以下の表を参照してください。

|項目|記入・設定例|
|--|--|
|Project Types|Classification|
|Classification Types|Multilabel|
|Domain|General|

![](images/1-portal-create-project.png)
