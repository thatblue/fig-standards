このドキュメントは英語とPHPの学習を目的とした、PHP-FIGとは無関係の**個人によるPSR-1の日本語訳**です。
もし誰かの参考になれば嬉しいですが、翻訳の品質は保証できませんので厳密な解釈を必要とする場合は必ず[原文のドキュメント](https://www.php-fig.org/psr/psr-1/)を当たってください。
誤訳等あればご指摘いただけると嬉しいです。

This document is Japanese translation of PSR-1 by individual person who do not related to PHP-FIG.
If you require strict interpretation, should refer [original document](https://www.php-fig.org/psr/psr-1/).

# 基本コーディング標準

このセクションは共有されたPHPのコードを高レベルに相互運用する上で考慮すべき標準的なコーディング要素から構成されています。

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[PSR-4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md

このドキュメントにある "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY",  "OPTIONAL"のキーワードは[RFC 2119(IPAによる日本語訳)][]で説明されている通りに解釈してください。

[RFC 2119(IPAによる日本語訳)]: https://www.ipa.go.jp/security/rfc/RFC2119JA.html

## 1. 概要

- ファイルでは`<?php`タグと`<?=`タグのみを使用しなければいけません(MUST)。

- ファイルではBOMなしのUTF-8のみを使用しなければいけません(MUST)。

- ファイルはシンボル(クラス、関数、定数など)の宣言*か*副作用のある処理(出力の生成や.iniの設定など)の*いずれか*である必要があり(SHOULD)、両方を含まないほうが望ましいです(SHOULD NOT)。

- 名前空間とクラスについてはオートロードに関するPSR([[PSR-0], [PSR-4]])に従わなければいけません(MUST)。

- クラス名は`StudlyCaps`(スタッドリーキャプス)で宣言しなければいけません(MUST)。

- クラス定数は大文字とアンダースコアの区切り文字で宣言しなければいけません(MUST)。

- メソッド名は`camelCase`(キャメルケース)で宣言しなければいけません(MUST)。

## 2. ファイル

### 2.1. PHPタグ

PHPコードでは長い形式の `<?php ?>` タグか、短縮系のechoタグ `<?= ?>` を使用しなければならず(MUST)、他の形式のタグは使用してはいけません(MUST NOT)。

### 2.2. 文字エンコード

PHPのコードではBOMなしのUTF-8のみを使用しなければいけません(MUST)。

### 2.3. 副作用

ひとつのファイルの中では新しいシンボル(クラス、関数、定数など)の宣言と副作用のない処理か、副作用のある処理のいずれかのみを実行する必要があり(SHOULD)、両方を含まないほうが望ましいです(SHOULD NOT)。

「副作用」とはクラス、関数、定数の宣言とは直接関係のない、*単にファイルを読み込むだけで* 実行される処理のことを指します。

「副作用」には以下のようなものを含みますが、これらに限りません: 出力を生成するもの、明示的な`require`や`include`の使用、外部サービスへの接続、iniの設定変更、エラーや例外を発生させること、グローバル変数や静的変数の変更、ファイルの読み書きなど。

以下に宣言と副作用のある処理の両方を含んだファイルの例を示します。つまりは避けるべき例です。

~~~php
<?php
// 副作用: ini設定の変更
ini_set('error_reporting', E_ALL);

// 副作用: ファイルの読み込み
include "file.php";

// 副作用: 出力の生成
echo "<html>\n";

// 宣言
function foo()
{
    // function body
}
~~~

以下は副作用のある処理を含まない宣言の例です。つまりは見習うべき例です。

~~~php
<?php
// 宣言
function foo()
{
    // 関数の本文
}

// 条件つきの宣言は副作用のある処理ではありません
if (! function_exists('bar')) {
    function bar()
    {
        // 関数の本文
    }
}
~~~

## 3. 名前空間とクラスの名前

名前空間とクラスについてはオートロードに関するPSR([[PSR-0], [PSR-4]])に従わなければいけません(MUST)。

それは、全てのクラスはファイルに単独で存在し、最低1レベル(トップレベルのベンダー名)の名前空間に属しているということを意味します。

クラス名は`StudlyCaps`(スタッドリーキャプス)で宣言しなければいけません(MUST)。

PHPのバージョンが5.3以上の場合、正式の名前空間を使用しなければいけません(MUST)。

例えば、

~~~php
<?php
// PHP 5.3以上
namespace Vendor\Model;

class Foo
{
}
~~~

5.2.x以前のコードでは習慣的に`Vendor_`形式の接頭辞を使った、擬似的な名前空間を使用する必要があります(SHOULD)。

~~~php
<?php
// PHP 5.2.x以前
class Vendor_Model_Foo
{
}
~~~

## 4. クラスの定数、プロパティ、メソッド

「クラス(class)」という用語はクラス、インターフェース、トレイトの全てを指します。

### 4.1. 定数

クラス定数は大文字とアンダースコアの区切り文字のみで宣言しなければいけません(MUST)。以下に例を示します。

~~~php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
~~~

### 4.2. プロパティ

このガイドではプロパティ名に`$StudlyCaps`(スタッドリーキャプス)、`$camelCase`(キャメルケース)、`$under_score`(アンダースコア)のいずれかを推奨することを意図的に避けています。

どんな命名の慣習を用いる場合でも、合理的なスコープの中では一貫性を保っている必要があります(SHOULD)。スコープの範囲はベンダー、パッケージ、クラス、メソッドのような区切りとなります。

### 4.3. メソッド

メソッド名は`camelCase()`(キャメルケース)で宣言しなければいけません(MUST)。