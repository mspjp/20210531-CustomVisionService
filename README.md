<h1> MS Tech Camp #7 </br>
Custom Vision Serviceで画像分類AIを作ろう！</h1>

[MS Tech Camp #7](https://mspjp.connpass.com/event/) で使用するハンズオン資料です。

Custom Vision Service で画像分類モデルを構築し、 APIを用いて送信した画像の分類結果を受け取ります。

## 1. Custom Vision Service プロジェクトを作成する

1-1 ブラウザで [Azureポータル](https://portal.azure.com/)を開いて、Azure for Studentsが関連付けられたMicrosoftアカウントでサインインを行います。

1-2 画面上部の検索バーで「Custom Vision」を入力し、現れた候補の中のCustom Visionアイコンをクリックする。

![](images/1-azure-portal.png)

1-3 「作成」というページに移るので、ここでモデルの計算リソースの情報を入力していきます。まずはリソースグループの新規作成を行います。

![](images/1-rg-create.png)

1-4 「図のように」リソースの名前、場所と価格レベルを設定した後、画面下部にある「確認および作成」のボタンをクリックし、さらに遷移先で「作成」をクリックします。ここでデプロイが終わるまでお待ちください。

![](images/1-rg-create-2.png)

1-2 次にブラウザで [Custom Vision Service ポータル](https://www.customvision.ai/) を開きます。 その後、[サインイン] を選択します。

1-3 サインインを求められたら、先ほどと同じMicrosoft アカウントを使用してサインインします。 このアプリにユーザー情報へのアクセスを許可するように求められたら、[はい] をクリックし、プロンプトが表示されたらサービス使用条件に同意します。

1-4 [NEW PROJECT] をクリックして、新しいプロジェクトを作成します。

![](images/1-portal-click-new-project.png)

1-5 [Create new project] ダイアログで、プロジェクトに "Artworks" という名前を付けます。

1-6 このプロジェクトに使用する リソース を選択します。先ほどのリソース作成が成功している場合、ここのリストでは自動的に表示されていると思います。 リソースをまだ用意していない場合は、[create new] を選択して新しいリソースを作成します。

1-7 必要事項を記入します。以下の表を参照してください。

|項目|記入・設定例|
|--|--|
|Project Types|Classification|
|Classification Types|Multilabel|
|Domain|General \[A2]|

## 2. タグを付けた画像をアップロードする

今回のハンズオンでは、ピカソ、ポロック、レンブラントの有名な絵画の画像を Artworks プロジェクトに追加します。 Custom Vision Service がある画家を別の画家から区別することを学習できるように、画像にタグを付けます。

2-1 作成した Artworks プロジェクトで、サイド パネルの [Tag] の右側にあるプラス記号 [+] を選択します。

![](images/2-add-tags.png)

2-2 [Create a new tag](新しいタグの作成) というダイアログ ボックスが表示されます。 タグ名フィールドに「絵画」と入力して、[保存] を選択します。 この操作により、タグ リストに "絵画" タグが作成されます。 さらに追加しましょう。

2-3 手順 1 と 2 を繰り返し、値が ピカソ、ポロック、レンブラント のタグを追加します。 完了すると、タグ リストは次のようになります。

![](images/2-tag-list.png)

プロジェクト内の画像のうちこれらの各タグでタグ付けされたものの数はまだ 0 になっています。 プロジェクトに画像をいくつか追加してタグを割り当てましょう。

2-4 ハンズオンの画像リソースが格納されている [cvs resources.zip](https://github.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/raw/master/cvs-resources.zip) をダウンロードし、ローカル コンピューターに解凍します。

2-5 ポータルに戻り、[Add images](画像の追加) を選択して、プロジェクトに画像を追加します。

![](images/)

2-6 手順 4 でローカルにダウンロードした cvs-resources フォルダーで、"Artists\Picasso" フォルダーに移動します。

2-7 "Artists\Picasso" 内のすべてのファイルを選択し、[開く] を選択します。

![](images/2-fe-browse-picasso-01.png)

2-8 [Image upload](画像のアップロード) ダイアログが開き、アップロードしたすべての画像のサムネイルが表示されます。 [My Tags](マイ タグ) フィールドを選択すると、これらの画像を割り当てることができるタグのドロップダウンが開きます。

![](images/2-upload-picasso-tags.png)

2-9 "絵画" タグと "ピカソ" タグを選択し、[Upload 7 files](7 個のファイルをアップロード) を選択してアップロードを終了します。

2-10 アップロードした画像が Artworks プロジェクトに含まれること、およびタグ リストが更新されて "ピカソ" と "絵画" タグが 7 個の画像に付けられたことが示されていることを確認します。

![](images/2-portal-tagged-01.png)

2-11 [Add images](画像の追加) を選択して、モジュール リソースの "Artists\Rembrandt" フォルダーにあるすべての画像を選択します。 それらに "絵画" と "レンブラント" というタグを付け、[Upload 6 files](6 個のファイルをアップロード) を選択してプロジェクトにアップロードします。

![](images/2-upload-rembrandt.png)

2-12 プロジェクトにピカソの画像と共にレンブラントの画像が表示され、タグの一覧に "レンブラント" と表示されることを確認します。

![](images/2-portal-tagged-02.png)

2-13 同じような方法で、ジャクソン ポロックの絵画を追加し、Custom Vision Service でポロックの絵画も認識できるようにします。 モジュール リソースの "Artists\Pollock" フォルダーですべての画像を選択し、"絵画" と "ポロック" というタグを付けて、プロジェクトにアップロードします。

## 3. モデルを訓練する

3-1 モデルをトレーニングするには、ページの上部にある [Train] を選択します。 モデルをトレーニングするたびに、新しいイテレーションが作成されます。 Custom Vision Service には、進捗を時間を追って比較できるいくつかのイテレーションが保持されます。

![](images/2-portal-click-train.png)

3-2 トレーニング プロセスが終わるまで待機します。次に、イテレーション 1 に対して示されるトレーニング統計を確認します。

結果では、モデルの精度のメジャーとして 精度 と 再現率 の 2 つが示されます。 モデルにピカソの画像 3 つとレンブラントの画像 3 つを渡したとします。 ピカソのサンプルのうち 2 つは "ピカソ" の画像として正しく識別されましたが、レンブラントのサンプルのうち 2 つはピカソとして誤って識別されたとします。 この場合、4 つの画像のうち 2 つが正しく識別されたため、精度 は 50% になります。 ピカソの 3 つの画像のうち 2 つが正しく識別されたため、再現率 のスコアは 67% になります。

![](images/2-portal-train-complete.png)

精度、再現率やF値に関するさらに詳しい説明は、こちらの[Qiita](https://qiita.com/FukuharaYohei/items/be89a99c53586fa4e2e4)などのウェブサイトを参照してください。

## 4. モデルをテストする

モデルのトレーニングが済んだので、今度はこれをテストします。 モデルに新しい画像を渡して、どれくらい正しく分類されるか確認します。

4-1 ページの上部にある [Quick Test](クイック テスト) を選択します。

![](images/4-portal-click-quick-test.png)

4-2 [ローカル ファイルを参照します] を選択してから、前にダウンロードしたモジュール リソース フォルダー内の "Quick Tests" フォルダーを参照します。 PicassoTest_01.jpg、[開く] の順に選択します。

![](images/4-portal-select-test-01.png)

4-3 [Quick Test](クイック テスト) ダイアログでテストの結果を確認します。 絵画がピカソである確率はどれくらいですか。 レンブラントやポロックである確率はどれくらいですか。

![](images/4-quick-test-result.png)

4-4 [Quick Test](クイック テスト) ダイアログを閉じます。 次に、ページの上部にある [予測] を選択します。

![](images/4-portal-select-predictions.png)

4-5 アップロードしたテスト画像を選択して、その詳細を表示します。 次に、ドロップダウンから [ピカソ] を選んで画像に "ピカソ" のタグを付け、[保存して閉じる] を選択します。

このようにテスト イメージをタグ付けすることによって、追加のトレーニング用イメージをアップロードすることなく、モデルを改良することができます。

![](images/4-tag-test-image.png)

4-6 別のクイック テストを実行し、今度は [Quick Test](クイック テスト) フォルダー内の FlowersTest.jpg という名前のファイルを使用します。 この画像にはピカソ、レンブラント、またはポロックである確率として低い値が割り当てられることを確認します。

## 5. モデルのAPIを呼び出す

このモデルは使用する準備ができており、特定の画家ごとに絵画をうまく識別することができます。 次に実際のアプリで利用するため、HTTP 経由でAPIを呼び出して、識別結果を取得してみましょう。

5-1 Custom Vision Service ポータルで Artworks* プロジェクトに戻り、[Performance] タブを選択します。

![](images/5-performance-tab.png)

5-2 [Publish] を選択して最新の反復を発行します。

5-3 [Prediction URL](予測 URL) を選択します。 呼び出しを行うのに必要な情報のダイアログ ボックスが表示されます。

![](images/5-portal-prediction-url.png)

ダイアログ ボックスで示されているように、予測エンドポイントを呼び出して画像の URL を渡すことできます。 生の画像を要求の本文でエンドポイントに渡すこともできます。

このダイアログ ボックスの 3 つの情報を書き留めておきます。

Prediction-Key(予測キー):すべての要求のヘッダーとしてこのキーを設定する必要があります。 このキーによってエンドポイントへのアクセス権が提供されます。
Request URL(要求 URL):ダイアログには 2 つの異なる URL が表示されます。 画像の URL を送信する場合は、末尾が /url の最初の URL を使用します。 要求の本文で生の画像を送信する場合は、末尾が /image の 2 番目の URL を使用しします。
Content-Type(コンテンツ タイプ):生の画像を送信する場合は、要求の本文に画像のバイナリ表現を設定し、コンテンツ タイプを application/octet-stream に設定します。 画像の URL を送信する場合は、本文に JSON として URL を設定し、コンテンツ タイプを application/json に設定します。

5-4 コマンドラインプロンプト（Windows）もしくはターミナル（Mac）で次のコマンドを実行します。 [endpoint-URL] は、最後の手順で保存した URL に置き換えます。 [Prediction-Key] は、最後の手順で保存した Prediction-Key の値に置き換えます。

```
curl [endpoint-URL] -H "Prediction-Key: [Prediction-Key]" -H "Content-Type: application/json" -d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/VanGoghTest_02.jpg'}"
```

コマンドが完了すると、次のスクリーンショットのような JSON 応答が表示されます。もし表示形式が見にくいと思われる方がいましたら、こちらの[JSONフォーマッター](https://jsonformatter.curiousconcept.com/)にコピーペーストして利用してみてください。

API は、モデル内のすべてのタグに対する確率を返します。 ご覧のように、"painting" tagName の値に 1 に近い確率が設定されているこの画像は、確かに絵画です。 しかし、モデルのトレーニングに使用したどの画家の絵画でもありません。

![](images/5-prediction-json.png)

5-5 上の要求本文内の URL を次の表にある URL に置き換え、さらに予測してみてください。

|画像|URL|
|--|--|
|![](images/picasso-test-02-thumb.jpg)|https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/PicassoTest_02.jpg|
|![](images/rembrandt-test-01-thumb.jpg)|https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/RembrandtTest_01.jpg|
|![](images/pollock-test-01-thumb.jpg)|https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/PollockTest_01.jpg|
