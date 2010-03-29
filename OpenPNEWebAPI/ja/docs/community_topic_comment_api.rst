================================================================
コミュニティトピックコメントに関する API (communityTopicComment)
================================================================

利用可能な機能
==============

.. toctree::
   :maxdepth: 1

   community_topic_comment_api/get_feed
   community_topic_comment_api/get_resource
   community_topic_comment_api/post_resource
   community_topic_comment_api/delete_resource

エントリポイント
================

コレクション URI
    http://example.com/api.php/feeds/communityTopicComment/communityTopic/{コミュニティトピックのID}

メンバ URI
    http://example.com/api.php/feeds/communityTopicComment/{コミュニティトピックイベントのID}

エントリ文書の書式
==================

コミュニティトピックコメントのエントリは以下のような Atom エントリ文書で表現されます。

::

  <?xml version="1.0" encoding="UTF-8" ?>
  <entry xmlns="http://www.w3.org/2005/Atom">
    <id>http://example.com/communityTopicComment/{community_topic_comment_id}</id>
    <published>2009-01-25T19:40:22+09:00</published>
    <updated>2009-01-25T19:40:22+09:00</updated>
    <content type="text">コミュニティトピックコメント本文</content>
    <author>
      <name>投稿したメンバーのニックネーム</name>
      <uri>http://example.com/member/{member_id}</uri>
    </author>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/communityTopicComment/{community_topic_comment_id}"/>
  </entry>


このエントリに含まれる要素の解説を以下に示します。

 * /entry/id : エントリを識別するための ID となる URN (省略可)
 * /entry/published : エントリの作成日時 (省略可)
 * /entry/updated : エントリの更新日時 (省略可)
 * /entry/content : エントリの本文
 * /entry/author : エントリの作成者の情報
    * /entry/author/name : エントリの作成者のニックネーム
    * /entry/auhtor/uri : エントリの作成者を識別するための URN
 * /entry/link[@rel="self"] : OpenPNE Web API でこのエントリを参照するための URI (省略可)

