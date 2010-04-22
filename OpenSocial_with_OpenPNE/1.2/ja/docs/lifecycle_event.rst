======================
ライフサイクルイベント
======================

`ライフサイクルイベント`_ とは、アプリに関する様々なイベントを、アプリ提供側に送る機能です。

現在の opOpenSocialPlugin では以下のイベントに対応しています。

* event.addapp
* event.removeapp

.. _`ライフサイクルイベント`: http://www.opensocial.org/Technical-Resources/opensocial-spec-v09/OpenSocial-Specification.html#rfc.section.4.1.7


宣言
----

ライフサイクルイベントを利用する場合はGadget XMLのModulePrefsタグ中に以下のような宣言をして下さい。

例::

  <Link rel="event.addapp" href="http://example.com/add" method="POST" />
  <Link rel="event.removeapp" href="http://example.com/remove" method="POST" />

属性は以下のようになります。

rel
  利用者がアプリが追加されたときに発生する **event.addapp** と、利用者がアプリの使用を終了するために削除したときに発生する **event.removeapp** を利用することができます。(必須)
href
  通知を受け付ける有効なURLを指定して下さい。(必須)
method
  HTTPリクエスト種類 (GETかPOSTを選択。指定しない場合はGETになります。)

通知についての注意事項
----------------------

ライフサイクルイベントは、イベント発生時に即時発行されるものではなく、SNS設置者が :ref:`設定<setup>` した間隔で通知されます。間隔や１度に通知される量についてはSNS設置者の告知を確認してください。

通知リクエストの内容
--------------------

各イベント通知リクエストでは、Authorizationリクエストヘッダに :ref:`署名<signature>` が付加されます。第三者からの不正な通知を受け付けないために必ず署名の検証を行って下さい。

それぞれのイベントには以下のパラメータが付加されています。

event.addapp
~~~~~~~~~~~~

eventtype
  "event.addapp"固定
id
  アプリの使用を開始したメンバーのID
from
  アプリをどこから追加したか。ギャラリーから追加した場合"gallery"、招待されて追加した場合は"invite"
invite_from
  招待されて追加した場合の招待者のメンバーID

event.removeapp
~~~~~~~~~~~~~~~

eventtype
  "event.removeapp"固定。
id
  アプリの使用を終了するために削除したメンバーのID
