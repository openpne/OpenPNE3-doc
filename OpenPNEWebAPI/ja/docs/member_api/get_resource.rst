.. _member_api_get_resource:

========================
メンバー::エントリの取得
========================

リクエスト
==========

リクエストの方法
----------------

メンバ URI に対して GET リクエストをおこなう。

::

  GET /api.php/feeds/member/{member_id} HTTP/1.1
  Host: example.com

リクエストパラメータ
--------------------

なし

レスポンス
==========

レスポンスヘッダ
----------------

HTTPステータスコード 200 を返す。

::

  HTTP/1.1 200 Ok
  Content-Type: application/atom+xml; charset=utf-8

レスポンス本文
--------------

XML 文書を返す。

::

  <?xml version="1.0" encoding="UTF-8" ?>
  <entry xmlns="http://www.w3.org/2005/Atom">
    <id>http://example.com/member/{member_id}</id>
    <published>2009-01-23T16:21:12+09:00</published>
    <updated>2009-01-23T16:21:12+09:00</updated>
    <title type="text">メンバーのニックネーム</title>
    <content type="xhtml">
      <div xmlns="http://www.w3.org/1999/xhtml">
        <div id="sex">性別(男性|女性)</div>
        <div id="birthday">誕生日(yyyy-mm-dd)</div>
        <div id="self_intro">自己紹介文</div>
      </div>
    </content>
    <author>
      <email>example@example.com</email>
    </author>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/member/{member_id}"/>
    <link rel="alternate" type="text/html" href="http://example.com/member/{member_id}"/>
    <link rel="alternate" href="http://example.com/mobile_frontend.php/member/{member_id}"/>
    <link rel="enclosure" href="メンバー画像URL" />
  </entry>

