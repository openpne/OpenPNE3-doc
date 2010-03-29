====================================
コミュニティに関する API (community)
====================================

利用可能な機能
==============

.. toctree::
   :maxdepth: 1

   community_api/get_feed
   community_api/get_resource
   community_api/post_resource
   community_api/put_resource
   community_api/delete_resource

エントリポイント
================

コレクション URI
    http://example.com/api.php/feeds/community

メンバ URI
    http://example.com/api.php/feeds/community/{コミュニティのID}

エントリ文書の書式
==================

コミュニティのエントリは以下のような Atom エントリ文書で表現されます。

::

  <?xml version="1.0" encoding="UTF-8" ?>
  <entry xmlns="http://www.w3.org/2005/Atom">
    <id>http://example.com/community/{community_id}</id>
    <published>2009-01-25T19:40:22+09:00</published>
    <updated>2009-01-25T19:40:22+09:00</updated>
    <title type="text">コミュニティタイトル</title>
    <content type="text">コミュニティ本文</content>
    <author>
      <name>投稿したメンバーのニックネーム</name>
      <uri>http://example.com/member/{member_id}</uri>
    </author>
    <link rel="edit" type="application/atom+xml" href="http://example.com/api.php/feeds/community/{community_id}"/>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/community/1"/>
    <link rel="alternate" type="text/html" href="http://example.com/community/{community_id}"/>
    <link rel="alternate" href="http://example.com/mobile_frontend.php/community/{community_id}"/>
  </entry>

このエントリに含まれる要素の解説を以下に示します。

 * /entry/id : エントリを識別するための ID となる URN (省略可)
 * /entry/published : エントリの作成日時 (省略可)
 * /entry/updated : エントリの更新日時 (省略可)
 * /entry/title : エントリのタイトル
 * /entry/content : エントリの説明文
 * /entry/author : エントリの作成者の情報
    * /entry/author/name : エントリの作成者のニックネーム
    * /entry/auhtor/uri : エントリの作成者を識別するための URN
 * /entry/link[@rel="edit"] : OpenPNE Web API でこのエントリを編集するための URI (省略可)
 * /entry/link[@rel="self"] : OpenPNE Web API でこのエントリを参照するための URI (省略可)
 * /entry/link[@rel="alternate" and @type="text/html"] : PC 版でこのエントリを閲覧するための URL (省略可)
 * /entry/link[@rel="alternate" and not(@type)] : 携帯版でこのエントリを閲覧するための URL (省略可)

