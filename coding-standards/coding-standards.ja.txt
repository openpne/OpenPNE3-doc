=================================
OpenPNE3 PHP 標準コーディング規約
=================================

概要
====

このドキュメントはコードの書式に関するガイドラインを、 OpenPNE3 に貢献する個人またはチームに示すためのものです。

OpenPNE3 プラットフォームに含まれるコードは必ずこのコーディング規約に従わなければなりません。 OpenPNE3 プラグインに含まれるコードは必ずしもこの規約に従わなくても構いませんが、他の開発者のために準拠しておくことを推奨します。

謝辞
----

このドキュメントは `Zend Framework PHP 標準コーディング規約`_ と `The Doctrine ORM Framework の Coding Standards`_ をベースに、 `symfony の Coding Standards`_ の内容を盛り込んだものです。

有用なドキュメントを公開されている各プロジェクトに厚く御礼申し上げます。

.. _`Zend Framework PHP 標準コーディング規約`: http://zendframework.com/manual/ja/coding-standard.html
.. _`The Doctrine ORM Framework の Coding Standards`: http://www.doctrine-project.org/documentation/manual/1_1/en/coding-standards
.. _`symfony の Coding Standards`: http://trac.symfony-project.org/wiki/HowToContributeToSymfony#CodingStandards

PHP ファイルの書式
==================

全般
----

PHP コードのみからなるファイルでは、終了タグ ("?>") は決して含めてはいけません。これは必須なものではなく、 終了タグを省略することで、ファイルの最後にある空白文字が出力に影響することを防ぎます。 

インデント
----------

インデントは半角スペース 2 文字でおこない、タブ文字は用いません::

  <?php
   
  class opFoo
  {
    public function bar()
    {
      return true;
    }
  }

行の長さ
--------

1 行の長さを 80 文字までにすることを目指しましょう。すなわち、 コードの長さを現実的な範囲で 80 文字までにおさめるよう努力すべきです。 しかしながら、場合によっては少々長くなってしまってもかまいません。 PHP コードの行の長さは、最大 120 文字までにするようにしましょう。 

行末
----

行末の扱いは、標準的な Unix テキストファイルの方式にあわせます。 行末は、ラインフィード (LF) のみにしなければなりません。 この文字のコードは 10、あるいは 16 進形式で 0x0A となります。

Macintosh のようなキャリッジリターン (CR) (0x0D) や Windows のようなキャリッジリターンとラインフィードの組み合わせ (CRLF) (0x0D, 0x0A) を使用しないでください。 

命名規約
========

クラス
------

クラス名は必ず "op" というプレフィックスをつけなければなりません。

クラス名は英数字のみを含み、アンダースコアは許可されません。クラス名が複数の単語から成り立つ場合は、単語の最初の文字を大文字にしてください（これは「キャメルケース」と呼ばれています）。

インターフェース
----------------

インターフェースの規約は通常のクラスに従ってください（前項を参照してください）。

なお、名前は "Interface" という語で終わるようにしなければなりません。

関数とメソッド
--------------

メソッド名はキャメルケースでなければなりません。

関数定義はヘルパー関数を除いて認められません。関数名は PHP 本体に従ってアンダースコアを使用しなければなりません (`PHP Coding Standards`_ (英語) も参照してください)。

.. _`PHP Coding Standards`: http://cvs.php.net/viewvc.cgi/php-src/CODING_STANDARDS?view=co

変数
----

変数名に含めることができるのは英数字のみです。アンダースコアを使用してはいけません。数字を含めることは可能ですが、ほとんどの場合はお勧めしません。変数名も "camelCaps" 方式に従わなければなりません。

冗長であることは歓迎されます。変数名は実際の言葉と同じくらい冗長であるべきです。 "$i" や "$n" などの短すぎる名前は小規模なループを除いて嫌われます。ループが 20 行以上にわたる場合、より説明的な名前の変数が求められます。

定数
----

定数は英数字とアンダースコアの両方が許容されます。アルファベットはすべて大文字でなければなりません。可読性のために、単語はアンダースコアで区切られていなければなりません。

定数はクラスメンバとして "const" によって定義されなければなりません。原則として "define" によるグローバルスコープでの定義は認められません。

レコードのカラム
----------------

すべてのカラムは小文字で構成されなければならず、複数の語からなる単語でアンダースコアを用いることが推奨されます。

外部キーは [テーブル名]_[カラム名] というフォーマットでなければなりません。

オプションやパラメータ
----------------------

しばしば配列のキーとして登場するオプションやパラメータは、小文字とアンダースコアを用いなければなりません。

ファイル名
----------

すべてのファイルにおいて、使用可能な文字は英数字、アンダースコア、ダッシュ文字 ("-") のみです。空白文字は使用できません。

PHP コードを含むすべてのファイルの拡張子は ".php" でなければなりません。またファイルがクラスかインターフェースを含む場合は ".class.php" で終わらなければなりませんが、モデルスクリプトは例外です。

プラグイン
----------

すべての OpenPNE プラグインの名前はキャメルケースであり、 "op" ではじまり "Plugin" で終わらなければなりません。

プラグインが認証のためのものであるならば、プラグイン名には "Auth" という語を含むべきです。

コーディングスタイル
====================

PHP コードの境界
----------------

PHP コードは常に完全な標準 PHP タグによって区切られなければならず、省略形 ("<? ?>" や "<?= ?>") は認められません。PHP コードのみからなるファイルでは、終了タグは決して含めてはいけません。

文字列
------

文字列リテラル
++++++++++++++

文字列がリテラルである（変数展開などを含んでいない）場合、シングルクオートで文字列を囲まなければなりません。

シングルクオートを含む文字列リテラル
++++++++++++++++++++++++++++++++++++

リテラル文字列にシングルクオートが含まれている場合、ダブルクオートの使用が許されます。

変数展開
++++++++

文字列内での変数展開は認められません。

代わりに文字列結合か sprintf() 関数を使用してください::

  $newString = $string.' is good.';
  $newString = sprintf('%s is good.', $string);

文字列結合
++++++++++

文字列は "." 演算子によって結合されます。 "." 演算子の前後にスペースを加えてはなりません::

  $openpne = 'OpenPNE'.' is '.' a '.' SNS '.' Engine.';

"." 演算子で文字列結合をおこなう際、可読性のために文を複数行にわたって記述することが許されています。そのような場合は 2 行目以降の行頭にスペースを入れ、各行の "." 演算子が最初の行の "=" 演算子と同じ位置にくるようにしなければなりません::

  $sql = "SELECT id, name FROM user "
       . "WHERE name = ? "
       . "ORDER BY name ASC";

配列
----

数値添字配列
++++++++++++

添字として負の数は許可されておらず、 0 以上の数から使用することができます。しかしながら、常に 0 からはじめるようにすることを推奨します。

array 構文を使用して添字配列を宣言する場合、可読性向上のために要素を区切るカンマのあとにスペースを入れなければなりません::

  $sampleArray = array('OpenPNE', 'SNS', 1, 2, 3);

array 構文を使用して複数行にまたがる配列を宣言することもできます。その場合、最初の要素を次の要素からはじめ、配列を宣言した位置からさらに一段インデントした位置で要素をそろえ、以降のすべての要素を同じインデントで記述するようにします。閉じ括弧はそれ単体でひとつの行に記述してインデント量は配列の宣言と同じ位置にあわせます::

  $sampleArray = array(
    1, 2, 3,
    $a, $b, $c,
    56.44, $d, 500,
  );

この宣言を使用する際は、配列の最後の要素の後にもカンマをつけておくようにしましょう。 そうすることで、配列に新たな要素を追加したときにパースエラーが発生する危険性を 少なくすることができます。

連想配列
++++++++

連想配列を array で宣言する場合、適宜改行を入れて複数行にします。最初の要素を次の要素からはじめ、配列を宣言した位置からさらに一段インデントした位置で要素をそろえ、以降のすべての要素を同じインデントで記述するようにします。閉じ括弧はそれ単体でひとつの行に記述してインデント量は配列の宣言と同じ位置にあわせます。可読性のために、代入演算子 "=>" の位置は揃えておくべきです::

  $sampleArray = array(
    'first'  => 'firstValue',
    'second' => 'secondValue',
  );

この宣言を使用する際は、配列の最後の要素の後にもカンマをつけておくようにしましょう。 そうすることで、配列に新たな要素を追加したときにパースエラーが発生する危険性を少なくすることができます。

クラス
------

開始波括弧は常にクラス名の下に置かれなければなりません。

すべてのクラスは PHPDocumentor 形式のドキュメントブロックを有していなければなりません。

すべての条件を満たすクラス定義は以下の通りです::

 /**
  * Documentation here
  */
  class opSampleClass 
  {
    // entire content of class
    // must be indented 2 spaces
  }

関数およびメソッド
------------------

宣言
++++

メソッドを宣言する際には、常に private, protected, public のいずれかの修飾子を用いてアクセス範囲を指定しなければなりません。

クラスと同様、波括弧は関数名の次に書かなければなりません。関数名と引数定義用の開始括弧の間にはスペースを入れてはいけません。

クラス内の関数宣言の例として適切なものを示します::

  /**
   * Documentation Block Here
   */
   class Foo 
   {
    /**
     * Documentation Block Here
     */
     public function bar() 
     {
       // entire content of function
       // must be indented 2 spaces
     }
   }

値の参照渡しはメソッドの宣言時のみ許されます。 

return 文の直前には可読性向上のために空行を入れるべきです::

  public function isBar()
  {
    $flag = true;
     
    if ($flag)
    {
      $this->someThingToDo();
       
      return $flag;
    }
     
    return false;
  }

使用方法
++++++++

関数の引数を指定する際は、引数を区切るカンマの後に空白をひとつ入れます。

引数として配列を受け取る関数については、関数コールの中に array 構文を含め、それを複数行に分けることもできます。

プロパティ
----------

プロパティを宣言する際には、常に private, protected, public のいずれかの修飾子を用いてアクセス範囲を指定しなければなりません。

制御構造
--------

制御構造では条件を指定する括弧の前に空白をひとつ入れなければなりません。

開始の波括弧は常に条件文の次の行に書かれます。終了の波括弧は常に改行して独立して記述されます。波括弧の中では空白 2 文字でインデントをおこないます。

条件文の開始の括弧の直後や終了の括弧の直前にはスペースを入れてはなりません::

  if ($foo == 1)
  {
    // body
  }
  elseif ($foo == 2)
  {
    // body
  }
  else
  {
    // body
  }

PHP では場合によっては、これらの文で波括弧が必要ないこともあります。 しかし、このコーディング規約では、このような例外を認めません。 "if"、"elseif" あるいは "else" 文では、常に波括弧を使用しなければなりません。

"switch" 文の中身は、空白 2 文字の字下げを使用します。 各 "case" 文の中身は、さらに 2 文字ぶん字下げします::

  switch ($case)
  {
    case 1:
      break;
    default:
      break;
  }

switch 文の default は、 決して省略してはいけません。 

コメント
--------

C 言語形式のコメント (`/* */`) と標準 C++ コメント (//) のどちらも使用可能です。 Perl/shell 形式のコメント (#) は使用するべきではありません。

すべての標準 C++ コメントはスペースからはじまるべきです。

変数のチェック
--------------

変数が null かどうかを用いる場合は is_null() 関数を用いてください::

  if (is_null($var))
  {
    echo '$var is null.';
  }

変数と値を比較する際は最初に値を置き、場合によっては型チェックもおこなってください::

  if (1 === $var)

インラインドキュメント
----------------------

ドキュメントの書式
++++++++++++++++++

ドキュメントブロック ("docblocks") は、phpDocumentor と互換性のある書式でなければなりません。 phpDocumentor の書式については、このドキュメントの対象範囲外です。 詳細な情報は http://phpdoc.org/  を参照してください。

すべてのクラスファイルは各クラスの直前に「クラスレベル」の docblock を含めなければなりません。以下に docblock の例を示します。 

クラスレベルの docblock
+++++++++++++++++++++++++

すべてのクラスは、最低限これらの phpDocumentor タグを含むドキュメントブロックを記述しなければなりません。

 * クラスについての一行の説明文
 * @author アノテーション
 * @package アノテーション。この値は OpenPNE かプラグイン名でなければなりません

以下に最小限のクラスレベルの docblock の例を示します::

    /**
     * Short description for class
     *
     * @package OpenPNE
     * @author  John Smith <jsmith@example.com>
     */

Copyright and License
=====================

::

  Copyright (c) 2005-2009, Zend Technologies USA, Inc.
  All rights reserved.

  Redistribution and use in source and binary forms, with or without modification,
  are permitted provided that the following conditions are met:

      * Redistributions of source code must retain the above copyright notice,
        this list of conditions and the following disclaimer.

      * Redistributions in binary form must reproduce the above copyright notice,
        this list of conditions and the following disclaimer in the documentation
        and/or other materials provided with the distribution.

      * Neither the name of Zend Technologies USA, Inc. nor the names of its
        contributors may be used to endorse or promote products derived from this
        software without specific prior written permission.

  THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
  ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
  WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
  DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
  ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
  (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
  LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
  ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
  (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
  SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
