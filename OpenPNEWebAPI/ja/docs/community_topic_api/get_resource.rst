.. _community_topic_api_get_resource:

====================================
コミュニティトピック::エントリの取得
====================================

リクエスト
==========

リクエストの方法
----------------

メンバ URI に対して GET リクエストをおこなう。

::

  GET /api.php/feeds/communityTopic/{community_topic_id} HTTP/1.1
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
    <id>http://example.com/communityTopic/{community_topic_id}</id>
    <published>2009-01-25T19:40:22+09:00</published>
    <updated>2009-01-25T19:40:22+09:00</updated>
    <title type="text">コミュニティトピックタイトル</title>
    <content type="text">コミュニティトピック本文</content>
    <author>
      <name>投稿したメンバーのニックネーム</name>
      <uri>http://example.com/member/{member_id}</uri>
    </author>
    <link rel="edit" type="application/atom+xml" href="http://example.com/api.php/feeds/communityTopic/{community_topic_id}"/>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/communityTopic/1"/>
    <link rel="alternate" type="text/html" href="http://example.com/communityTopic/{community_topic_id}"/>
    <link rel="alternate" href="http://example.com/mobile_frontend.php/communityTopic/{community_topic_id}"/>
  </entry>

