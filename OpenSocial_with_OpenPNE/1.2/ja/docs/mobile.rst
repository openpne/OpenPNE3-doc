==============
モバイルアプリ
==============

概要
====

.. image:: images/mobile.png

1. 利用者の携帯端末からSNSへHTTPリクエストが送られます。

2. SNSからアプリ提供者のサーバへHTTPリクエストが送られます。このとき、パラメタとしてメンバーIDとアプリID、HTTPヘッダとして **署名** と個体識別番号などを除いた端末情報が含まれます。このリクエストを受け取ったサーバでは :ref:`署名<signature>` を検証する必要があります。

3. 必要な場合 :ref:`OpenSocial RESTful API<restful_api>` を利用してソーシャル情報（メンバーの情報やフレンドなど) の取得を試みます。

4. アプリ提供者のサーバ利用者の携帯端末上に表示するHTMLをレスポンスすると、SNSはその内容を出力します。この時に、レスポンスするHTMLの文字エンコードはUTF-8として下さい。SNSでの出力時に利用者の携帯端末に対応する文字エンコードに変換されます。

宣言
====

モバイル版を利用する場合は、Gadget XMLに以下のようなmobile Viewを追加します。

Gadget XML::

  <?xml version="1.0" encoding="UTF-8"?>
  <Module>
    <ModulePrefs title="Test Mobile Application" />
    <Content type="url" view="mobile" href="#START_URL#" />
  </Module>

*#START_URL#* はアプリを開いたときに、最初にリクエストをするURLを指定して下さい。

アプリはPCと同じように追加することができます。

SNSからアプリ提供者へ渡る情報
=============================

リクエストパラメタには以下が追加されます。

opensocial_app_id
  実行中のアプリケーションのID

opensocial_owner_id
  現在アクセスしているメンバーのID

リクエストヘッダには以下が追加されます。

Authorization
  :ref:`署名<signature>` とそれを検証するための情報

User-Agent
  個体識別番号を除いた利用者のUser-Agent

X-Up-*
  X-Up-Subnoを除いた利用者の端末情報

X-Jphone-*
  X-Jphone-Uidを除いた利用者の端末情報

X-S-Display-Info
  ブラウザの表示に関する情報


画面遷移
========

画面遷移はアプリ提供者サーバ上のURLをパラメータに指定したリンクにより行うことができます。
SNSでのセッションを維持するために以下のようにして下さい。

例::

  <a href="?url=#URL#">画面遷移リンク</a>

*#URL#* はアプリ提供者サーバ上のURLをURLエンコードしたものです。

遷移すると指定したURLにリクエストを送ります。

FORMでも同様のURL指定を行って下さい。

招待ページ
==========

アプリを招待するページにリンクする場合は以下のように行って下さい。
出力時に適切なURLに置き換えられます。

例::

  <a href="invite:friends">招待する</a>

オプションとして、callbackパラメータを指定することができます。
これは招待完了時に遷移するURLになります。これを省略した場合は、スタートURLに遷移します。

位置情報の取得
==============

アプリにユーザの位置情報を送信させたい場合は、a要素（アンカー）またはform要素に遷移先URLとして以下のようなURLを用いて下さい。
出力時に適切なURLに置き換えられます。

GPS

  location:gps?callback=#URL#

簡易位置情報

  location:cell?callback=#URL#

指定できるパラメータ

callback
  省略可能。省略時はスタートURLが使われる。
  位置情報取得後に遷移するURL

なお、a要素のhref属性や、form要素のaction属性にパラメータを含める場合には、パラメータの値はRFC3986形式でURLエンコードを施す必要があります。
以下に記述例を示します。

GET

  <a href="location:gps?callback=http%3A%2F%2Fexample.com%2Fsample.php">位置情報を取得する</a>

POST

  <form action="location:gps" method="post">
  <input type="hidden" name="callback" value="http://example.com/sample.php">
  <input type="submit" value="位置情報を取得する">
  </form>

位置情報の取得に成功すると、callbackパラメータで指定したURL（位置情報取得後に遷移するURL）にクエリ文字列として次の datum, lat, lon パラメータが付加されます。

取得できるパラメータ

datum
  取得した位置情報の測地系。世界測地系1984を用いるため、常に ``wgs84`` （世界測地系1984であることを示す値）が返る。

lat
  緯度。
  例: ``+34.44.36.02``

lon
  経度。
  例: ``+135.26.44.35``

つまり、次のようなリクエストクエリを含むURLに遷移されることになります。

  http://example.com/sample.php?datum=wgs84&lat=%2B34.44.36.02&lon=%2B135.26.44.35
