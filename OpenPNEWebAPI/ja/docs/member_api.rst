=============================
メンバーに関する API (member)
=============================

利用可能な機能
==============

.. toctree::
   :maxdepth: 1

   member_api/get_feed
   member_api/get_resource

エントリポイント
================

コレクション URI
    http://example.com/api.php/feeds/member

メンバ URI
    http://example.com/api.php/feeds/member/{メンバーのID}

エントリ文書の書式
==================

メンバーのエントリは以下のような Atom エントリ文書で表現されます。

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

このエントリに含まれる要素の解説を以下に示します。

 * /entry/id : エントリを識別するための ID となる URN (省略可)
 * /entry/published : エントリの作成日時 (省略可)
 * /entry/updated : エントリの更新日時 (省略可)
 * /entry/title : エントリのニックネーム
 * /entry/content : エントリのプロフィール情報
    * /entry/content/div/div[@id="プロフィールを表す識別子"] : プロフィール設定値
 * /entry/author : このエントリのメンバーの情報
    * /entry/author/email : このメンバーのメールアドレス (管理者もしくは本人を除いて取得不可)
 * /entry/link[@rel="self"]/@href : OpenPNE Web API でこのエントリを参照するための URI (省略可)
 * /entry/link[@rel="alternate" and @type="text/html"]/@href : PC 版でこのエントリを閲覧するための URL (省略可)
 * /entry/link[@rel="alternate" and not(@type)]/@href : 携帯版でこのエントリを閲覧するための URL (省略可)
 * /entry/link[@rel="enclosure"]/@href : このエントリの画像 (省略可)

