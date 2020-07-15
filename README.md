# E2info Laravel Coding Rule

Laravelでのシステム開発時のコーディング規約と方針です

## 対象読者

* イーツー・インフォのエンジニア、パートナー

## 基本的な規約

### 前提

一定水準のもとでコミュニケーションを取る必要があるため、以下の知識を有する状態でプロジェクトに参加する必要があります。

* PSR
    * https://www.php-fig.org/psr/ 
* リーダブルコード
    * https://www.amazon.co.jp/dp/4873115655
* IPA 安全なウェブサイトの作り方
    * https://www.ipa.go.jp/security/vuln/websecurity.html
* Laravelのドキュメント
    * https://laravel.com/docs/
* うまくメソッド名を付けるための参考情報
   * https://qiita.com/KeithYokoma/items/2193cf79ba76563e3db6
* うまくクラス名を付けるための参考情報
   * https://qiita.com/KeithYokoma/items/ee21fec6a3ebb5d1e9a8

### 規約

#### 原則

* 基本的にPSRに準拠（ACCEPTED）
    * https://www.php-fig.org/psr/ 
* 変数名、関数名はlowerCamelとする
* 各ファイルにはdeclare(strict_types=1);を記載する

```
<?php
declare(strict_types=1);
```

* タブ・スペースは.editorconfigに従う
* 利用しないuse文は削除する ```ctrl + option + o```
* .phpファイルはphpStormの整形ツールでフォーマットしてください
    * ルールは標準のまま
        * phpStormの標準ルールとPSRで一部矛盾する部分はphpStormの標準ルールを優先します
    * とりあえず何も考えずにphpStormでフォーマットすれば基本OKのはず
    * CIが導入されているプロジェクトではCIで自動フォーマット

#### タイプヒンティング

* 引数の型を定義する

```
findCoupon($date) {
↓
findCoupon(Carbon $date) {
```

* 戻り値の型を定義する

```
function getUserID(){
↓
function getUserID() : int {
```

* プロパティの型を定義する

```
public $workerService;
↓
public WorkerService $workerService;
```

### 方針

#### レイヤー

MVCで以下の構造

* app
    * Http
        * Controllers
* Models (*Entities)
* Services (*Use Cases)

*はクリーンアーキテクチャとのマッピング

#### ライブラリ選定

* マルチテナントの要件がある場合、Tenancyを採用します。ただし運用コストが増える可能性もあるため、導入すべきかどうか必ず検討してください。

#### 実装方針

* 一般的な更新処理はPRG(Post/Redirect/Get)パターンを適用する
* 関数名にset/getを利用しない。ただし、インタフェースが明示的である場合のみ利用可能
* and/&&は、&&に統一

```
if(a and b)
↓
if(a && b)
```

* postリクエストに対してはFormRequestを利用する
* 関数/変数の名前は汎用的なものにしない

```
×
function findList();

○
function findReservations();
```

* viewへの値の渡し方はwithで統一する(compactは禁止とする)

```
×
return view('user.list', compact('users'));

○
return view('user.list')->with(['users' => $users]);
```

* 変数名***Data/***Listは限定的なスコープ内で気をつけて利用すること

```
×
$resultData = $data->shutoku->shori();
// 
// 
// （10行くらい略）
// 
// 
// このあたりにくると、$resultDataがなんのデータなのかよくわからない

△
// スコープが限定されているため理解できる
$resultData = $data->shutoku->shori();
// （1行なにかの処理）
return $resultData;

○
$withdrawalUsers = $data->shutoku->shori();
// （10行くらい略）
// （10行くらい略）
// （10行くらい略）
// （10行くらい略）
// （10行くらい略）
// （10行くらい略）
// （10行くらい略）
// （10行くらい略）
// （10行くらい略）
// （10行くらい略）
// $withdrawalUsers=退会済みのユーザー一覧ということが変数名からわかる
```

* マジックナンバーは変数化して利用すること。何らかの理由がある場合、必ずコメントを書くこと。

```
// レアケース＆工数削減のため商品コード直書き（クライアント合意済み）
$ignoreItemCodes = ['1001', '1002'];
```

* コード内にコメントを残さないこと。何らかの理由がある場合、必ずコメントを書くこと。

```
// 一時的なキャンペーン用のコードのため現在は無効。ただし、将来再利用する可能性があるためコメント化しておく
/*
$xxx = $nanika->no->shori();
*/
```

![イーツー・インフォロゴ](https://raw.githubusercontent.com/e2info/e2info-warehouse/master/images/logo/logo100x100_transparent.png)
