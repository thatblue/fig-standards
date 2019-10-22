このドキュメントは英語とPHPの学習を目的とした、PHP-FIGとは無関係の**個人によるPSR-12の日本語訳**です。
もし誰かの参考になれば嬉しいですが、翻訳の品質は保証できませんので厳密な解釈を必要とする場合は必ず[原文のドキュメント](https://www.php-fig.org/psr/psr-12/)を当たってください。
誤訳等あればご指摘いただけると嬉しいです。

This document is Japanese translation of PSR-12 by person who not related PHP-FIG.
If you require strict interpretation, should refer [original document](https://www.php-fig.org/psr/psr-12/).

# 拡張コーディングスタイルガイド

このドキュメントにある "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY",  "OPTIONAL"のキーワードは[RFC 2119 (IPAによる日本語訳)][]で説明されている通りに解釈してください。

[RFC 2119 (IPAによる日本語訳)]: https://www.ipa.go.jp/security/rfc/RFC2119JA.html

## 概要

この仕様書はコーディングスタイルガイドである[PSR-2][]を継承、拡張し、置き換えるもので、基本的なコーディング標準であるPSR-1の遵守を必要とします。

[PSR-2][]と同様に、この仕様書の目的は複数の作者によるコードリーディングの認識の摩擦を減らすことにあります。これはPHPのコードをどのようにフォーマットするかのルールと期待する内容のセットを共有することで実現します。このPSRはコーディングスタイルツールが実装できる決まったやり方を提供するよう努力しており、それによってプロジェクトが遵守を宣言し、開発者が複数のプロジェクト間で関連づけることが容易になります。様々な作者が複数のプロジェクトを超えてコラボレーションするとき、それらすべてのプロジェクトで使用されるガイドラインが1組あると役立ちます。したがって、このガイドの恩恵はルールそのものではなく、このルールを共有することにあります。

[PSR-2][]は2012に承認され、それ以降コーディングスタイルガイドに影響する変更がいくつもPHPに加えられてきました。[PSR-2]は執筆時点でPHPの機能を非常に包括的にカバーしていましたが、新しい機能については様々な解釈が可能でした。そこで、このPSRはよりモダンなコンテキストにおいて利用可能になった機能におけるPSR-2の内容を明確にするよう努力し、PSR-2の正誤表を作ります。

### 過去のバージョン

この資料全体を通して、適用するプロジェクトで使うPHPのバージョンに存在しないものは無視しても構いません(MAY)。

### 例

この例は後述するいくつかのルールを含んだ簡単な概要です。

~~~php
<?php

declare(strict_types=1);

namespace Vendor\Package;

use Vendor\Package\{ClassA as A, ClassB, ClassC as C};
use Vendor\Package\SomeNamespace\ClassD as D;

use function Vendor\Package\{functionA, functionB, functionC};

use const Vendor\Package\{ConstantA, ConstantB, ConstantC};

class Foo extends Bar implements FooInterface
{
    public function sampleFunction(int $a, int $b = null): array
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // method body
    }
}
~~~

## 2. 一般的なルール

### 2.1 基本的なコーディング仕様

コードは[PSR-1]で説明されている全てのルールに従わなければいけません(MUST)。

PSR-1にある「スタッドリーキャプス」という用語は一番はじめの文字を含む、それぞれの単語の最初の文字を大文字にしたもの(パスカルケース)として解釈しなければなりません(MUST)。

### 2.2 ファイル

すべてのPHPファイルは行の末尾をUnix式のLF(ラインフィード)で終わらせなければいけません(MUST)。

すべてのPHPファイルはLFで終わる空ではない行で終わらせなければいけません(MUST)。

PHPのコードのみで構成されるファイルは閉じタグの `?>`を省略しなければいけません(MUST)。

### 2.3 行

1行の文字数にハードリミットを設けてはいけません(MUST NOT)。

1行の文字数のソフトリミットは120文字でなければいけません(MUST)。

1行の文字数は80字を超えないことが望ましく(SHOUD NOT)、それを越える場合は80文字を超えないように複数の行に分割した方が望ましいです(SHOUD)。

行末に空白文字があってはいけません(MUST NOT)。

明示的に禁止されている場合を除き、可読性を上げる目的で関係のあるコードブロックを示すために空行を挿入しても構いません(MAY)。

1つの行に複数の処理を記述してはいけません(MUST NOT)。

### 2.4 インデント

コードは各レベルごとにスペース4つで表現しなければなりません(MUST)、またインデントにタブを使用してはいけません(MUST NOT)。

### 2.5 キーワードと型

全てのPHPの予約語と型 [[1]][keywords][[2]][types]は小文字で記述しなければなりません(MUST)。

将来のPHPにおいて新しく追加される型やキーワードは小文字で記述しなければなりません(MUST)。

型のキーワードは短いものを使わなければなりません(MUST)。
例: `boolean`の代わりに`bool`、`integer`の代わりに`int`

## 3. 宣言文、名前空間、インポート文

PHPファイルのヘッダはいくつかのブロックから成り立つ可能性があります。もし存在するならば、以下に示すそれぞれのブロックは1つの空行で分けられていなければならず(MUST)、またそれぞれのブロックの中に空行を含んではいけません(MUST NOT)。それぞれのブロックは以下に示す順に並んでいなければいけません(MUST)が、関係のないものは省略しても構いません。

* PHPの開始タグ `<?php`
* ファイルレベルのphpDocブロック
* 1つ以上の宣言文
* ファイルのネームスペース宣言
* 1つ以上のクラス`use` インポート文
* 1つ以上の関数`use`インポート文
* 1つ以上の定数`use`インポート文
* 以降はコード

ファイルにHTMLとPHPが混ざって含まれている場合、上記の内容は引き続き適用可能ですが、その場合はたとえ残りのコード部分にPHPの閉じタグが含まれていて、HTMLとPHPが混在していてもヘッダ部分をファイルの冒頭に配置しなければいけません(MUST)。

PHPの開始タグがファイルの最初の行に含まれる場合、PHPの開始・終了タグの外側でマークアップの記述がない限り、その行は開始タグ以外の記述を含まないようにしなければいけません(MUST)。

インポート文は先頭をバックスラッシュで始めず(MUST)、常に完全修飾名で記述してください。

以下にこの節全体の記述例を示します。

~~~php
<?php

/**
 * This file contains an example of coding styles.
 */

declare(strict_types=1);

namespace Vendor\Package;

use Vendor\Package\{ClassA as A, ClassB, ClassC as C};
use Vendor\Package\SomeNamespace\ClassD as D;
use Vendor\Package\AnotherNamespace\ClassE as E;

use function Vendor\Package\{functionA, functionB, functionC};
use function Another\Vendor\functionD;

use const Vendor\Package\{CONSTANT_A, CONSTANT_B, CONSTANT_C};
use const Another\Vendor\CONSTANT_D;

/**
 * FooBar is an example class.
 */
class FooBar
{
    // ... additional PHP code ...
}

~~~

名前空間をグループ化するときに2階層を超えてはいけません(MUST NOT)。したがって、以下に示すものが許容されるもっとも深い階層となります。

~~~php
<?php

use Vendor\Package\SomeNamespace\{
    SubnamespaceOne\ClassA,
    SubnamespaceOne\ClassB,
    SubnamespaceTwo\ClassY,
    ClassZ,
};
~~~

また、以下のものは許容されません。

~~~php
<?php

use Vendor\Package\SomeNamespace\{
    SubnamespaceOne\AnotherNamespace\ClassA,
    SubnamespaceOne\ClassB,
    ClassZ,
};
~~~

strict_typesの定義をPHPタグの外でマークアップの記述を含むファイルで宣言したい場合、その宣言はファイルの先頭でPHPの開始タグ、strict_typeの宣言、PHPの閉じタグを含んでいなければいけません(MUST)。

例えば、

~~~php
<?php declare(strict_types=1) ?>
<html>
<body>
    <?php
        // ... additional PHP code ...
    ?>
</body>
</html>
~~~

declare文は空白文字を含まないようにしなければならず(MUST)、かつ`declare(strict_types=1)`と正確に一致していなければいけません(MUST)。(セミコロンの区切り文字は任意)

declareブロック文は下記の通りのフォーマットで書かなければいけません(MUST)。括弧やスペースの位置に注意してください。

~~~php
declare(ticks=1) {
    // some code
}
~~~

## 4. クラス、プロパティ、メソッド

「クラス(class)」という用語はクラス、インターフェース、トレイトの全てを指します。

閉じ中括弧のあと、同じ行にコメントや文が続いてはいけません(MUST NOT)

クラスをインスタンス化するとき、コンストラクタに渡す引数がなかったとしても括弧はかならず必要です(MUST)。

~~~php
new Foo();
~~~

### 4.1 継承と実装

`extends`と`implements`のキーワードはクラス名の宣言と同じ行にしなければいけません(MUST)。

クラス開始の中括弧は独立した行に記述されなければならず(MUST)、終了の中括弧はクラス本文の次の行に記述する必要があります(MUST)。

開始の中括弧は独立した行に記述されなければならず(MUST)、かつその前後に空行があってはいけません(MUST NOT)。

終了の中括弧は独立した行に記述されなければならず(MUST)、かつその前に空行があってはいけません。

~~~php
<?php

namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constants, properties, methods
}
~~~

インターフェースの`implements`のリストと`extends`は複数行に分割することができ(MAY)、その場合はそれぞれの行を1段階インデントしてください。その場合、最初の要素を次の行に置く必要があり(MUST)、1行あたりインターフェースを1つずつ書く必要があります(MUST)。

~~~php
<?php

namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // constants, properties, methods
}
~~~

### 4.2 トレイトの利用

`use`キーワードを用いてトレイトを実装する時はクラス本文の中、開始の中括弧の次の行に定義しなければいけません(MUST)。

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName
{
    use FirstTrait;
}
~~~

複数のトレイトをインポートする場合、1行に1つずつ、それぞれ`use`キーワードを単独で用いて宣言しなければいけません(MUST)。

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;
use Vendor\Package\SecondTrait;
use Vendor\Package\ThirdTrait;

class ClassName
{
    use FirstTrait;
    use SecondTrait;
    use ThirdTrait;
}
~~~

`use`を用いたインポート以降に何も記述がない場合、終了の中括弧は`use`文の直後になければいけません(MUST)。

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName
{
    use FirstTrait;
}
~~~

それ以外の場合、`use`インポート文の後は空行が必要です(MUST)。

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName
{
    use FirstTrait;

    private $property;
}
~~~

`insteadof`や`as`演算子を使用する場合は下記に示すようにインデント、スペース、改行を使用してください。

~~~php
<?php

class Talker
{
    use A, B, C {
        B::smallTalk insteadof A;
        A::bigTalk insteadof C;
        C::mediumTalk as FooBar;
    }
}
~~~

### 4.3 プロパティと定数

すべてのプロパティにおいてアクセス権の定義は必須です(MUST)。

定数のアクセス権をサポートしているPHPバージョン(PHP7.1以上)ではアクセス権の定義は必須です(MUST)。

プロパティの定義に`var`キーワードを使用してはいけません(MUST NOT)。

1つの文で2つ以上のプロパティを定義してはいけません(MUST NOT)。

protectedやprivateであることを示すためにプロパティ名をアンダースコア1つで始めてはいけません(MUST NOT)。すなわち、アンダースコアのプレフィックスは明示的に何の意味も持ちません。

型宣言とプロパティ名の間は1つのスペースで区切られていなければいけません(MUST)。

プロパティの宣言は以下のようになります。

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public $foo = null;
    public static int $bar = 0;
}
~~~

### 4.4 メソッドと関数

すべてのメソッドにおいてアクセス権の定義は必須です(MUST)。

protectedやprivateであることを示すためにメソッド名をアンダースコア1つで始めてはいけません(MUST NOT)。すなわち、アンダースコアのプレフィックスは明示的に何の意味も持ちません。

メソッドや関数の定義部分において、メソッド名や関数名の後にスペースがあってはいけません(MUST NOT)。開始の中括弧は独立した行にある必要があり(MUST)、終了の中括弧はメソッドや関数本文の次の行にある必要があります(MUST)。引数列開始の括弧の後にスペースがあってはならず(MUST NOT)、また引数列終了の括弧の前にスペースがあってはいけません(MUST NOT)。

メソッドの定義は以下のような形式となります。括弧、カンマ、スペース、中括弧の配置に注意してください。

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
~~~

関数の定義は以下のような形式となります。括弧、カンマ、スペース、中括弧の配置に注意してください。

~~~php
<?php

function fooBarBaz($arg1, &$arg2, $arg3 = [])
{
    // function body
}
~~~

### 4.5 メソッドと関数の引数

引数のリストでは、カンマの前にスペースがあってはならず(MUST NOT)、カンマの後には1つのみスペースがなければいけません(MUST)。

メソッドと関数の引数において、デフォルト値を持つものは引数リストの末尾になければいけません(MUST)。

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public function foo(int $arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
~~~

引数リストは複数の行に分割しても構いません(MAY)、その場合は分割した行を1段階ずつインデントしなけければならず(MUST)、1行につき引数1個ずつ記載しなければいけません(MUST)。

引数リストを複数の行に分割す流場合、引数列の閉じ括弧と本文開始の中括弧はそれのみの同じ行に配置し、間に1個のみスペースがなければいけません(MUST)。

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
~~~

戻り値の型宣言が存在する場合、コロンの後に1個のスペースを置き、その後に型宣言を記載しなければいけません(MUST)。コロンと型定義は引数リストの閉じ括弧とスペースを挟まず同じ行に置かなければいけません(MUST)。

~~~php
<?php

declare(strict_types=1);

namespace Vendor\Package;

class ReturnTypeVariations
{
    public function functionName(int $arg1, $arg2): string
    {
        return 'foo';
    }

    public function anotherFunction(
        string $foo,
        string $bar,
        int $baz
    ): string {
        return 'foo';
    }
}
~~~

nullを許容する場合、`?`と型の間にスペースを挟んではいけません(MUST NOT)。

~~~php
<?php

declare(strict_types=1);

namespace Vendor\Package;

class ReturnTypeVariations
{
    public function functionName(?string $arg1, ?int &$arg2): ?string
    {
        return 'foo';
    }
}
~~~

リファレンス渡しの`&`記号を使用する場合は上記に示した例のように引数の前に置き、記号の後にスペースを置いてはいけません(MUST NOT)。

可変引数の`...`演算子と引数名の間にスペースがあってはいけません(MUST NOT)。

```php
public function process(string $algorithm, ...$parts)
{
    // processing
}
```

リファレンス渡しの記号と可変引数の演算子を組み合わせて使用する場合、それらの間にスペースを置いてはいけません(MUST NOT)。

```php
public function process(string $algorithm, &...$parts)
{
    // processing
}
```

### 4.6 `abstract`と`final`と`static`

`abstract`や`final`の宣言を行う場合、アクセス権の定義の前に記述しなければいけません(MUST)。

`static`の宣言を行う場合、アクセス権の定義の後に記述しなければいけません(MUST)。

~~~php
<?php

namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // method body
    }
}
~~~

### 4.7 メソッドと関数の呼び出し

メソッドや関数の呼び出しを行う場合、メソッド・関数名と引数の開始の括弧の間にスペースを挟んではならず(MUST NOT)、開始の括弧の後にスペースを置いてはならず(MUST NOT)、終了の括弧の前にスペースを置いてはいけません(MUST NOT)。引数リストにおいて、それぞれのカンマの前にスペースを置いてはならず(MUST NOT)、またカンマの後に1つだけスペースを置かなければいけません(MUST)。

~~~php
<?php

bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
~~~

引数リストは複数の行に分割しても構いません(MAY)、その場合は分割した行を1段階ずつインデントしなけければいけません(MUST)。その場合、先頭の要素を次の行に置かなければならず(MUST)、1行につき引数1個ずつ記載しなければいけません(MUST)。(無名関数や配列のような場合に)1つの引数のみを複数行に分割する場合は他の要素の分割に影響しません。

~~~php
<?php

$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
~~~

~~~php
<?php

somefunction($foo, $bar, [
  // ...
], $baz);

$app->get('/hello/{name}', function ($name) use ($app) {
    return 'Hello ' . $app->escape($name);
});
~~~

## 5. 制御構造

制御構造に関する一般的なスタイルルールは以下の通りです。

- 制御構造キーワードの後には1つのスペースが必要です(MUST)
- 開始の括弧の後にスペースを置いてはいけません(MUST NOT)
- 終了の括弧の前にスペースを置いてはいけません(MUST NOT)
- 終了の括弧と開始の中括弧の間には1つのスペースを挟まなければいけません(MUST)
- 本文は1段階インデントを下げなければいけません(MUST)
-  本文は開始の中括弧の次の行から始めなくてはいけません(MUST)
- 終了の中括弧は本文の次の行に置かなければいけません(MUST)

それぞれの本文は中括弧で囲まれていなければいけません(MUST)。この規約により、構造がどのように見えるかが標準化され、新しい行が追加されるときに問題が発生する可能性が低くなります。

### 5.1 `if`と`elseif`と`else`

`if`文のスタイルは以下の通りです。括弧、スペース、中括弧の配置に注意してください。また、`else`と`elseif`は前ブロックのボディの閉じ中括弧と同じ行に記述してください。

~~~php
<?php

if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
~~~

すべての制御キーワードが1つの単語に見えるように、`else if`の代わりに`elseif`を用いる必要があります(SHOULD)。

括弧の中の式は複数の行に分割することができ(MAY)、そのときは分割した行を最低1段階インデントしてください。その際、最初の条件文は括弧の次の行から開始しなければいけません(MUST)。条件文終了の括弧と本文開始の中括弧はスペース1個を挟んで同じ行に記述しなければいけません(MUST)。条件文の間の論理演算子は行頭か行末のいずれかに統一して配置しなければいけません(MUST)。

~~~php
<?php

if (
    $expr1
    && $expr2
) {
    // if body
} elseif (
    $expr3
    && $expr4
) {
    // elseif body
}
~~~

### 5.2 `switch`と`case`

`switch`文は以下の通りに記述してください。括弧、スペース、中括弧の配置に注意してください。`case`文は`switch`より1段階インデントを下げて記述しなければならず(MUST)、`break`キーワード(もしくは他の中断するキーワード)は`case`文の内容と同じレベルのインデントでなければいけません(MUST)。`case`文の内容が空ではなく、意図的に後続の文を実行させたい場合は`// no break`のようなコメントを書かなければいけません(MUST)。

~~~php
<?php

switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
~~~

括弧の中の式は複数の行に分割することができ(MAY)、そのときは分割した行を最低1段階インデントしてください。その際、最初の条件文は括弧の次の行から開始しなければいけません(MUST)。条件文終了の括弧と本文開始の中括弧はスペース1個を挟んで同じ行に記述しなければいけません(MUST)。条件文の間の論理演算子は行頭か行末のいずれかに統一して配置しなければいけません(MUST)。

~~~php
<?php

switch (
    $expr1
    && $expr2
) {
    // structure body
}
~~~

### 5.3 `while`と`do while`

`while`文のスタイルは以下の通りです。括弧、スペース、中括弧の配置に注意してください。

~~~php
<?php

while ($expr) {
    // structure body
}
~~~

括弧の中の式は複数の行に分割することができ(MAY)、そのときは分割した行を最低1段階インデントしてください。その際、最初の条件文は括弧の次の行から開始しなければいけません(MUST)。条件文終了の括弧と本文開始の中括弧はスペース1個を挟んで同じ行に記述しなければいけません(MUST)。条件文の間の論理演算子は行頭か行末のいずれかに統一して配置しなければいけません(MUST)。

~~~php
<?php

while (
    $expr1
    && $expr2
) {
    // structure body
}
~~~

同様に、`do while`文のスタイルは以下の通りです。括弧、スペース、中括弧の配置に注意してください。

~~~php
<?php

do {
    // structure body;
} while ($expr);
~~~

括弧の中の式は複数の行に分割することができ(MAY)、そのときは分割した行を最低1段階インデントしてください。その際、最初の条件文は括弧の次の行から開始しなければいけません(MUST)。条件文の間の論理演算子は行頭か行末のいずれかに統一して配置しなければいけません(MUST)。

~~~php
<?php

do {
    // structure body;
} while (
    $expr1
    && $expr2
);
~~~

### 5.4 `for`

`for`文のスタイルは以下の通りです。括弧、スペース、中括弧の配置に注意してください。

~~~php
<?php

for ($i = 0; $i < 10; $i++) {
    // for body
}
~~~

括弧の中の式は複数の行に分割することができ(MAY)、そのときは分割した行を最低1段階インデントしてください。その際、最初の条件文は括弧の次の行から開始しなければいけません(MUST)。条件文終了の括弧と本文開始の中括弧はスペース1個を挟んで同じ行に記述しなければいけません(MUST)。

~~~php
<?php

for (
    $i = 0;
    $i < 10;
    $i++
) {
    // for body
}
~~~

### 5.5 `foreach`

`foreach`文のスタイルは以下の通りです。括弧、スペース、中括弧の配置に注意してください。

~~~php
<?php

foreach ($iterable as $key => $value) {
    // foreach body
}
~~~

### 5.6 `try`, `catch`, `finally`

`try-catch-finally`ブロックのスタイルは以下の通りです。括弧、スペース、中括弧の配置に注意してください。

~~~php
<?php

try {
    // try body
} catch (FirstThrowableType $e) {
    // catch body
} catch (OtherThrowableType | AnotherThrowableType $e) {
    // catch body
} finally {
    // finally body
}
~~~

## 6. 演算子

演算子のスタイルルールは項数(オペランドの数)でグルーピングされます。

演算子の周囲にスペースを使用することが認められており、可読性を目的として複数のスペースを使用しても構いません(MAY)。

ここで説明されていない演算子はすべて未定義のままです。

### 6.1. 単項演算子

インクリメント/デクリメント演算子は演算子とオペランドの間にスペースを挟んではいけません(MUST NOT)。

~~~php
$i++;
++$j;
~~~

型キャスト演算子は括弧の内側にスペースがあってはいけません(MUST NOT)。

~~~php
$intValue = (int) $input;
~~~

### 6.2. 二項演算子

全ての[代数演算子][]、[比較演算子][]、[代入演算子][]、[ビット演算子][]、[論理演算子][]、[文字列演算子][]、[型演算子][]は前後に1個以上のスペースを置かなければいけません(MUST)。

~~~php
if ($a === $b) {
    $foo = $bar ?? $a ?? $b;
} elseif ($a > $b) {
    $foo = $a + $b * $c;
}
~~~

### 6.3. 三項演算子

三項演算子は`?`と`:`それぞれの前後に1個以上のスペースを置かなければいけません(MUST)。

~~~php
$variable = $foo ? 'foo' : 'bar';
~~~

三項演算子の真ん中のオペランドが省略されている場合、演算子は二項の[比較演算子][]と同様のスタイルルールを適用しなければいけません(MUST)。

~~~php
$variable = $foo ?: 'bar';
~~~

## 7. 無名関数(クロージャ)

無名関数は`function`キーワードの後にスペース1個を置いた後に定義し、`use`キーワードの前後にスペースを1個ずつ置かなければいけません(MUST)。

本文開始の中括弧は同じ行に配置しなければならず(MUST)、終了の中括弧は本文の次の行に配置しなければいけません(MUST)。

引数リストと変数リストの開始の括弧の後にスペースがあってはならず(MUST NOT)、終了の括弧の前にスペースがあってはいけません(MUST NOT)。

引数リストと変数リストにおいて、カンマの前にスペースを置いてはならず(MUST NOT)、カンマの後に1つスペースを置かなければいけません(MUST)。

無名関数の引数において、デフォルト値を持つものは引数リストの末尾になければいけません(MUST)。

戻り値の型宣言が存在する場合、通常の関数やメソッドのコーディングルールに従わなければいけません(MUST)。`use`キーワードが存在する場合、コロンは`use`リストの閉じ括弧の後に置き、閉じ括弧とコロンの間にスペースを挟まず配置しなければいけません(MUST)。

無名関数のスタイルは以下の通りです。括弧、スペース、中括弧の配置に注意してください。

~~~php
<?php

$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};

$closureWithArgsVarsAndReturn = function ($arg1, $arg2) use ($var1, $var2): bool {
    // body
};
~~~

引数と変数のリストは複数行に分割することができ(MAY)、その場合はそれぞれの行を1段階インデントしてください。その場合、最初の要素を次の行に置く必要があり(MUST)、1行あたりインターフェースを1つずつ書く必要があります(MUST)。

末尾のリスト(変数リストがある場合はそちら、なければ引数リスト)が複数行に分割されている場合、リスト終了の括弧と本文開始の中括弧はスペース1個を挟んで同じ行に記述しなければいけません(MUST)。

以下に引数リストと変数リストがそれぞれある場合とない場合の複数行に分割した例を示します。

~~~php
<?php

$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
~~~

関数やメソッドの引数として直接使用される場合にも同じルールが適用される点に注意してください。

~~~php
<?php

$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
~~~

## 8. 無名クラス

無名クラスは前述の無名関数と同様のガイドラインと原則に従わなければいけません(MUST)。

~~~php
<?php

$instance = new class {};
~~~

`implements`キーワードが改行されない限り、開始の中括弧は`class`のキーワードと同じ行にあっても構いません(MAY)。もしインターフェースの箇所で改行されている場合、中括弧は最後のインターフェースの直後に記載されなければいけません(MUST)。

~~~php
<?php

// Brace on the same line
$instance = new class extends \Foo implements \HandleableInterface {
    // Class content
};

// Brace on the next line
$instance = new class extends \Foo implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // Class content
};
~~~

[PSR-1]: http://www.php-fig.org/psr/psr-1/
[PSR-2]: http://www.php-fig.org/psr/psr-2/
[keywords]: http://php.net/manual/ja/reserved.keywords.php
[types]: http://php.net/manual/ja/reserved.other-reserved-words.php
[代数演算子]: http://php.net/manual/ja/language.operators.arithmetic.php
[代入演算子]: http://php.net/manual/ja/language.operators.assignment.php
[比較演算子]: http://php.net/manual/ja/language.operators.comparison.php
[ビット演算子]: http://php.net/manual/ja/language.operators.bitwise.php
[論理演算子]: http://php.net/manual/ja/language.operators.logical.php
[文字列演算子]: http://php.net/manual/ja/language.operators.string.php
[型演算子]: http://php.net/manual/ja/language.operators.type.php
