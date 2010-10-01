=========================================
OpenPNE3 セキュアコーディングガイドライン
=========================================

目次
====

.. contents::

概要
====

このドキュメントは、セキュリティを前提としたコーディングをおこなうために従うべきガイドラインを、 OpenPNE3 に貢献する個人またはチームに示すためのものです。

ユーザをなりすましから守る
==========================

セッション管理
--------------

ブルートフォースアタックを回避する
----------------------------------

パスワードの再確認
------------------

パスワード文字列生成のヒント
----------------------------

正しく入力値を検証する
======================

サーバサイドでの入力値検証
--------------------------

ホワイトリストやブラックリストの利用
++++++++++++++++++++++++++++++++++++

Null Byte Attack による制限の回避への対策
+++++++++++++++++++++++++++++++++++++++++

クライアントサイドでの入力値検証
--------------------------------

安全に HTML レスポンスを生成する
================================

XSS (Cross Site Scripting) 脆弱性
---------------------------------

XSS とは、攻撃者がウェブページに任意のコード (多くの場合は JavaScript) を挿入することのできる脆弱性です。

挿入されたコードは被害者のブラウザ上で実行されます。そのため、そのサイトにおいてクライアントがおこなうことのできる操作のほとんどを実行させることができます。

この脆弱性は、ユーザ入力値などの信頼できないデータを含む Web ページを動的に生成する際に、たとえば、その入力値が直接 HTML の構造に作用してしまう形で埋め込んだ場合などに発生します。

たとえば、以下のサンプルコードでは、 GET パラメータの name の値を HTML の一部として出力しています::

  <?php echo '<p>Hello, '.$_GET['name'].'!</p>';

このソースコードは、 `http://example.com/?name=Ebihara` のようにアクセスした場合に、以下のように出力されることを意図したものです::

  <p>Hello, Ebihara!</p>

しかし、 `http://example.com/?name=<strong>Ebihara</strong>` のようにして、入力値に HTML タグを含めてしまうと、この HTML がそのまま出力に挿入されてしまいます。これは、任意のスクリプトを挿入可能な状態にあるということを意味します::

  <p>Hello, <strong>Ebihara</strong>!</p>

HTML タグをそのまま反映させることを意図しているのでない限り、本来は以下のように出力されなければなりません::

  <p>Hello, &lt;strong&gt;Ebihara&lt;/strong&gt;!</p>

この例のように HTML にユーザ入力値を埋め込む場合の XSS への対策方法はよく知られていますが、動的に生成する JavaScript や画像、 Flash などの Web ブラウザが実行可能なコンテンツすべてについても、この脆弱性への対策を施す必要があります。

XSS による脅威
--------------

JavaScript などによってユーザのブラウザが実行可能なほとんどの操作をおこなうことができます。

もし XSS に脆弱であれば、マルウェアの配布サイトにユーザを連れて行ったり、ページ上に表示されている機密情報を流出させたりといったことができます。セッションクッキーを盗むこともできるので、攻撃者はユーザになりすましてログインすることもできます。

また、フィッシングの手口と組み合わせることで、攻撃者にユーザのパスワードを知られてしまう危険性が向上します。

非常に緊急度の高い脆弱性ですので、発覚してしまった場合は即座に対策を施すべきです。

HTML の生成
-----------

HTML の利用を制限したい入力値にある HTML 特殊文字 (&, <, >, ", ') を、出力時にエスケープする必要があります。

特殊文字が文字参照になるように適切にエスケープが施されていれば、特殊文字を利用して HTML の要素の内容に埋め込まれた入力値から HTML の構造を変更させることで XSS 攻撃を成立させることはできなくなります。

symfony のアクションを通じてテンプレートに渡された値は、明示的に無効にしていない限り、この文字参照へのエスケープの処理が自動的におこなわれます。

たとえば、以下のようなアクションを考えます::

  <?php
  
  class exampleActions extends sfActions
  {
    public function executeIndex(sfWebRequest $request)
    {
      $this->name = $request['name'];
    }
  }

アクションの $name プロパティに値を代入したことで、この $name の値をテンプレートから参照できるようになりました。

このときのリクエストパラメータ name の値が `<strong>Ebihara</strong>` だったとして、以下のようにしてテンプレートから出力しても、結果は正しくエスケープされた状態になります::

  <p>Hello, <?php echo $name ?>!</p>
  /* Output: <p>Hello, &lt;strong&gt;Ebihara&lt;/strong&gt;!</p> */

実はテンプレートからアクセスできる $name の値は、エスケープ済みの文字列というわけではありません。 symfony のアクションを介してテンプレートに変数をアサインすると、その変数の値は sfOutputEscaper でラッピングされます。ですので、アクションからテンプレートに渡された変数は、特別に許可された一部のクラスインスタンスを除き、実際には sfOutputEscaper およびその派生クラスのインスタンスになります。 sfOutputEscaper についての詳細は symfony の http://www.symfony-project.org/gentle-introduction/1_4/en/07-Inside-the-View-Layer#chapter_07_output_escaping を参照してください。

sfOutputEscaper のインスタンスは、アクションから渡された生の値を保持しており、 echo や . 演算子、関数などにより文字列として扱われると、保持している生の値をエスケープして返します。

これにより変数内の HTML 特殊文字のエスケープは適切におこなわれるようになりましたが、 HTML 属性値としてユーザ入力値を出力しようとする際に脆弱になることがあります::

  <p id=<?php echo $name ?>>Hello, <?php echo $name ?>!</p>

このとき $name の生の文字列が `Ebihara onmouseover=alert(0);` だった場合、以下のように p 要素の属性値が追加されてしまい、マウスカーソルを合わせるとスクリプトが実行されてしまいます::

  <p id=Ebihara onmouseover=alert(0);>Hello, Ebihara onmouseover=alert(0);!</p>

" や ' は sfOutputEscaper によってエスケープされるので、このようなケースでは、以下のように属性値を引用符で囲うことで、属性値を超えて入力値が反映されることはなくなります::

  <p id="Ebihara onmouseover=alert(0);">Hello, Ebihara onmouseover=alert(0);!</p>

引用符は ' でも構いませんが、 PHP において HTML 特殊文字のエスケープに用いられる htmlspecialchars() 関数は、第二引数に ENT_QUOTES を与えない限り ' をエスケープしないため、 ' がエスケープされていない状態の入力値が ' で囲まれた属性値として埋め込まれた場合に脆弱になります。 OpenPNE のデフォルト設定では sfOutputEscaper は ENT_QUOTES つきで htmlspecialchars() をコールしますが、原則として引用符には " を使用するべきです。

ただし、この対策をしても以下のような場合は依然として脆弱なことがあるので注意してください (対策方法は後述します)。

 * イベントハンドラを記述するような属性値 (onclick や onmouseover など) に入力値を埋め込む場合
 * 任意の要素の style 属性値
 * a 要素の href 属性値に入力値を埋め込む場合
 * img 要素の src 属性値に入力値を埋め込む場合

JavaScript の生成
-----------------

JavaScript に動的な値を埋め込む場合、 \ を付加することによって特定の文字をエスケープをすることがあります。

しかしながら、どのような文字をエスケープするべきなのかが明確ではありませんし、攻撃者はエスケープされそうな文字に対してさらに \ を付加することで、この対策を回避しようとすることがあります。そのため、エスケープに漏れが生じる可能性があります。

また、現在の文字エンコーディングにおいて不正な文字などと隣接させることによって \ によるエスケープは容易に回避することができます（※これは記憶を頼りに書いているのであとで検証）。

JavaScript に動的な値を文字列として埋め込む場合は以下のどちらかの手段を用いることを強く推奨します。

 1. 英数字以外の文字を \xHH のように置換する。
 2. HTML 要素の属性値や内容として動的な値を挿入し、 JavaScript から DOM を用いてその値を純粋な JavaScript の文字列として取ってくる。

特に、 2. の方法を用いることを推奨します。以下に例を示します::

  <input id="example" type="hidden" value="<?php echo $name ?>" />
  
  <script type="text/javascript"><![CDATA[
  alert(document.getElementById("example").value);
  //]]></script>

この方法であれば、 HTML の作法に基づいて動的に生成した値を埋め込み、 JavaScript からそれを文字列として取得するだけで済むので、動的に埋め込まれた値は常に JavaScript の文字列のまま保たれることになり、危険は生じえません。

CSS の生成
----------

CSS には expression() プロパティなどによりスクリプトを埋め込むことができます。ですので、 CSS に入力値を埋め込む場合も適切に配慮をおこなわなければなりません。

CSS でも \ による特定の文字のエスケープがおこなわれることがありますが、 JavaScript の場合と同様、避けるべきです。

英数字以外の文字を \xHH のように置換することで CSS に動的な値を、確実に文字列として利用できるようになります。

しかしながら、管理画面からの入力を除いて、入力値に基づいて CSS を生成することはなるべく回避することをお勧めします。

画像の生成
----------

Internet Explorer では、レスポンスヘッダ内の Content-Type のみならず、コンテンツの中身も確認した上で、最終的にそのレスポンス内容をどのような種類のコンテンツとして処理するべきか決定します。

たとえば、 Content-Type が image/gif であっても、レスポンスの内容が HTML であれば text/html として読み込んでしまいます。 (CAPEC-209: Cross-Site Scripting Using MIME Type Mismatch http://capec.mitre.org/data/definitions/209.html)

HTML として読み込まれた画像に JavaScript が埋め込まれていれば、ブラウザは当然にその JavaScript を実行してしまい、 XSS 脆弱性が成立してしまいます。

OpenPNE ではユーザのアップロードした画像を表示するために sfImageHandlerPlugin を用意しています。このプラグインで処理された画像は、一度 GD を通して画像を生成し直した上で表示されることになるため、画像以外の情報は除去された状態になり、安全に画像を表示することができます。

ユーザの画像アップロードを許す場合、その画像をそのまま表示するということはせずに、 sfImageHandlerPlugin もしくは他の手段を用いてから表示するようにしてください。

.. 文脈にあったエスケープを心がける
.. --------------------------------

安全に SQL を生成する
=====================

HTML の生成と同様、 SQL の生成にあたっても、ユーザ入力値など信頼できない値の取り扱いには注意が必要です。

ユーザ入力値を含んだ SQL 文を動的に生成する場合、その入力値によって、最終的に実行される SQL の構文を意図したものと違うものに変更されてしまう可能性があります。

これは SQL Injection と呼ばれている脆弱性です。この脆弱性が存在していると、攻撃者にデータベースに存在する情報の漏洩や改ざんを許してしまいます。

たとえば、以下のようなコードは SQL Injection に対して脆弱です::

  <?php
  // $pdo は PDO のインスタンス
  $pdo->query(sprintf('SELECT * FROM user WHERE username = "%s" AND password = "%s";', $_GET['username'], $_GET['password']));

`http://example.com/?username=jsmith&password=example` のような URL にアクセスがあった場合、このコードの意図通りに、以下の SQL 文が生成され、実行されます::

  SELECT * FROM user WHERE username = "jsmith" AND password = "example";

しかし、 `http://example.com/?username=jsmith%22;%20--%20&password=whatever` のような URL にアクセスすると、以下のクエリが実行されてしまいます (`--` 以降はコメント) ::

  SELECT * FROM user WHERE username = "jsmith"; -- " AND password = "whatever";

また、複数文の発行が許可されている場合には、 `http://example.com/?username=%22;%20DELETE%20FROM%20user;%20SELECT%20username%20AS%20dummy%20FROM%20user%20WHERE%20%22%22%20%3D%20%22&password=whatever` のような URL にアクセスされると、以下のように DELETE 文が発行されてしまいます::

  SELECT * FROM user WHERE username = "";
  DELETE FROM user;
  SELECT username AS dummy FROM user WHERE "" = "" AND password = "whatever";

OpenPNE で SQL Injection に対処するには、バインド機構を使用して SQL 文を生成するようにするのが一番よい解決方法です。

バインド機構とは、実際の値を埋め込む場所を記号 (プレースホルダ) で示した SQL 文をあらかじめ準備しておき、後からプレースホルダを実際の値に置き換えて SQL を構築する機構のことをいいます。バインド機構はプレースホルダから実際の値に置き換えるときに、実際の値を正しくエスケープします。

PDO はバインド機構に対応しているので、先に示したサンプルコードを以下のように変更することで、 SQL Injection からアプリケーションを守ることができます::

  <?php
  // $pdo は PDO のインスタンス
  $sth = $pdo->prepare('SELECT * FROM user WHERE username = ? AND password = ?;');
  $sth->execute(array($_GET['username'], $_GET['password']));

OpenPNE においては、自分で SQL 文を生成するすべての箇所で SQL Injection に対して配慮をおこなわなければなりません。 OpenPNE ではほとんどの場合直接 SQL 文を書かずに、 Doctrine の DQL 文を直接記述もしくは構築し、その DQL を SQL に変換して実行するということをおこなっていますが、 この DQL も以下のように誤った形で組み立ててしまうと、結局、 SQL Injection に脆弱になってしまいます::

  <?php
  Doctrine::getTable('User')->createQuery()
    ->where(sprintf('username = "%s" AND password = "%s"', $_GET['username'], $_GET['password']))
    ->execute();

このコードは、バインド機構を利用して DQL を組み立てるために、以下のように記述するべきです::

  <?php
  Doctrine::getTable('User')->createQuery()
    ->where('username = ? AND password = ?', array($_GET['username'], $_GET['password']))
    ->execute();

一方で、たとえば Doctrine_Table::find() メソッドに関しては、 SQL Injection に対して配慮して SQL 文が生成されるため、引数を渡す際に特別な配慮をおこなう必要はありません。ですが、 Doctrine_Table::findBySql() や Doctrine_Table::findByDql() といった SQL や DQL を自分で組み立てるようなメソッドを利用する場合には、やはり、 SQL Injection に対する配慮が求められることになります。

自分で SQL や DQL を組み立てる必要があり、 SQL Injection に対する配慮が必要なものとしては、たとえば以下のようなものがあります。

 * PDO 以外のデータベース関連拡張が提供する関数群
 * PDO::exec() や PDOStatement::execute() などクエリを実行する PDO のメソッド
 * Doctrine_Connection::fetchAll() など直接 SQL を実行する Doctrine_Connection のメソッド
 * Doctrine_RawSql
 * Doctrine_Query
 * Doctrine_Table::findBySql() など、自分で作成したクエリを元にレコードを取得するようなメソッド

また、バインド機構を利用したとしても、ユーザ入力値に基づいてカラム名などを動的に組み立てるような場合は、 SQL Injection に対して脆弱となります。できるだけそのようなコードは控えるようにするべきですが、それが難しい場合、必ず、動的に組み立てる箇所に対してエスケープやクオート処理を実施してください。

エスケープ等に使用できる Doctrine のメソッドとしては以下のようなものがあります。エスケープ等が必要な記号群やエスケープ手法などはデータベースエンジンによって異なります。そのため、独自処理を施すより、 Doctrine が用意しているメソッドを利用しておこなうことを強く推奨します。

 * Doctrine_Formatter::escapePattern()
 * Doctrine_Connection::quote()
 * Doctrine_Connection::quoteIdentifier()

安全に外部コマンドを実行する
============================

意図しないファイルへのアクセスを防ぐ
====================================

SNS 内情報を安全な形で保存する
==============================

アクセス権限の制限の回避を防ぐ
==============================

CSRF (Cross Site Request Forgries) 脆弱性
-----------------------------------------

フォームにおける対策
++++++++++++++++++++

アクション内での対策
++++++++++++++++++++

Ajax リクエストにおける対策
+++++++++++++++++++++++++++

アクションで認証を要求する
--------------------------

権限チェックの漏れを防ぐ
------------------------
