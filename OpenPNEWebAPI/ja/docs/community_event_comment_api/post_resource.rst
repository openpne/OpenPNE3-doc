.. _community_event_comment_api_post_resource:

============================================
コミュニティイベントコメント::エントリの追加
============================================

リクエスト
==========

リクエストの方法
----------------

コレクション URI に対して POST リクエストをおこなう。

::

  POST /api.php/feeds/communityEventComment/communityEvent/{community_event_id} HTTP/1.1
  Host: example.com
  Content-Type: application/atom+xml; charset=utf-8

リクエストパラメータ
--------------------

なし

リクエスト本文
--------------

以下のサンプルに登場する要素をすべて含めた XML 文書である必要がある。

::

  <?xml version="1.0" encoding="UTF-8" ?>
  <entry xmlns="http://www.w3.org/2005/Atom">
    <content type="text">コミュニティイベントコメント本文</content>
    <author>
      <name>投稿するメンバーのニックネーム</name>
      <uri>http://example.com/member/{member_id}</uri>
    </author>
  </entry>


レスポンス
==========

レスポンスヘッダ
----------------

HTTPステータスコード 201 を返す。

Location ヘッダには、新しく作成されたリソースのメンバ URI が含まれる。

::

  HTTP/1.1 201 Created
  Location: http://example.com/api.php/feeds/communityEventComment/{community_event_id}
  Content-Type: application/atom+xml; charset=utf-8

レスポンス本文
--------------

XML 文書を返す。

::

  <?xml version="1.0" encoding="UTF-8" ?>
  <entry xmlns="http://www.w3.org/2005/Atom">
    <id>http://example.com/communityEvent/{community_event_id}</id>
    <published>2009-01-25T19:40:22+09:00</published>
    <updated>2009-01-25T19:40:22+09:00</updated>
    <content type="text">コミュニティイベント本文</content>
    <author>
      <name>投稿したメンバーのニックネーム</name>
      <uri>http://example.com/member/{member_id}</uri>
    </author>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/communityEventComment/{community_event_comment_id}"/>
  </entry>

