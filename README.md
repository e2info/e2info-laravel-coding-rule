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

### 規約

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

### 方針

* なるべく関数名にset/getを利用しない。ただし、インタフェースが明確的である場合のみ利用可能
* 関数変数の名前は汎用的なものにしない(×getList、○getReservations)
* 引数・戻り値の型が決まっている場合は定義する

引数
```
findCoupon($date) {
↓
findCoupon(Carbon $date) {
```

戻り値

```
function getUserID(){
↓
function getUserID() : int {
```

* andと&&混在しそうなので&&に統一

```
if(a and b)
↓
if(a && b)
```


![イーツー・インフォロゴ](https://raw.githubusercontent.com/e2info/e2info-warehouse/master/images/logo/logo100x100_transparent.png)
