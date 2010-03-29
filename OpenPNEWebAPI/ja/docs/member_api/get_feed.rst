.. _member_api_get_feed:

==============================
メンバー::エントリの一覧の取得
==============================

リクエスト
==========

リクエストの方法
----------------

コレクション URI に対して GET リクエストをおこなう。

::

  GET /api.php/feeds/member HTTP/1.1
  Host: example.com

リクエストパラメータ
--------------------

* :ref:`q <search-query>` (optional)
* :ref:`updated-min <updated-query>` (optional)
* :ref:`updated-max <updated-query>` (optional)
* :ref:`published-min <published-query>` (optional)
* :ref:`published-max <published-query>` (optional)
* :ref:`start <start-query>` (optional)
* :ref:`max-results <max-requests-query>` (optional)
* :ref:`orderby <orderby-query>` (optional)
* :ref:`sortorder <sortorder-query>` (optional)
* uid (optional)
    - 携帯電話個体識別番号（完全一致）で絞り込みをおこなう
* email (optional)
    - 携帯電話メールドレス（完全一致）で絞り込みをおこなう

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

XML 文書を返す。取得した件数分 entry 要素を格納する。

::

  <?xml version="1.0" encoding="UTF-8" ?>
  <feed xmlns="http://www.w3.org/2005/Atom"
          xmlns:openSearch="http://a9.com/-/spec/opensearchrss/1.0/">
    <title>Members</title>
    <id>http://example.com/member/{member_id}</id>
    <updated>2009-01-28T11:23:55+09:00</updated>
    <generator uri="http://www.openpne.jp/">OpenPNE</generator>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/member"/>
    <link rel="http://schemas.google.com/g/2005#feed" type="application/atom+xml" href="http://example.com/api.php/feeds/member"/>
    <openSearch:totalResults>総件数</openSearch:totalResults>
    <openSearch:startIndex>取得を開始したエントリの番号</openSearch:startIndex>
    <openSearch:itemsPerPage>1ページあたりの件数</openSearch:itemsPerPage>
    <entry>
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
    <entry>
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
  </feed>
