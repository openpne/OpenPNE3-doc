.. _community_event_comment_api_delete_resource:

============================================
コミュニティイベントコメント::エントリの削除
============================================

リクエスト
==========

リクエストの方法
----------------

メンバ URI に対して DELETE リクエストをおこなう。

::

  DELETE /api.php/feeds/communityEventComment/{community_event_comment_id} HTTP/1.1
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

レスポンス本文
--------------

なし
