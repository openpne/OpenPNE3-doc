======================
GadgetとJavaScript API
======================

SNS上でOpenSocialアプリケーションを実現したい場合は、Gadgetを開発します。Gadgetはメタ情報とコンテンツ（JavaScriptやHTML）を記述Gadget XMLファイルによって実現します。

Gadget中では、OpenSocial JavaScript APIを利用することにより、SNS上のメンバー・フレンド情報などを取得することができ、それに基づいたソーシャルアプリケーションを開発することができます。

この章では、GadgetとJavaScript APIについて解説します。


基本構造とアプリの追加
======================

もっとも単純なGadget XMLは以下のようになります。

Gadget XML::

  <?xml version="1.0" encoding="UTF-8"?>
  <Module>
    <ModulePrefs title="Test Application">
      <Require feature="opensocial-0.8" />
    </ModulePrefs>
    <Content type="html">
    <![CDATA[
    こんにちは！
    ]]>
    </Content>
  </Module>

このファイルを、SNS上から読み取れるサーバ上から読み取れるようにします。OpenPNEの管理画面から、プラグイン設定 > opOpenSocialPluginの設定 > アプリ追加 にアクセスし、XMLファイルのURLを指定し、アプリを追加します。

アプリの追加に成功すると、アプリの情報画面が表示されます。

この画面からは、アプリを強制的に更新したり、削除することができます。


SNSメンバーは、アプリ > アプリギャラリー から、このアプリを追加することができます。


OWNER と VIEWER
===============

OpenSocialアプリケーションユーザ情報に関してOWNERとVIEWERという概念があります。

アプリをインストールしてをOWNERといい、そのアプリを閲覧している人をVIEWERといいます。

メンバー情報の取得
==================

Gadget中でのメンバーの情報の取得は次のようになります。


例::

  <?xml version="1.0" encoding="UTF-8" ?>
  <Module>
    <ModulePrefs title="Test Application">
      <Require feature="opensocial-0.8" />
    </ModulePrefs>
    <Content type="html">
    <![CDATA[
    <div id="console"></div>
    <script type="text/javascript">
      // リクエスト
      function request() {
        var req = opensocial.newDataRequest();

        req.add(req.newFetchPersonRequest(opensocial.IdSpec.PersonId.VIEWER), "get_viewer");
        req.add(req.newFetchPersonRequest(opensocial.IdSpec.PersonId.OWNER), "get_owner");

        req.send(response);
      }

      // レスポンス
      function response(data) {
        var viewer = data.get("get_viewer").getData();
        var owner = data.get("get_owner").getData();

        var html = "";
        html = viewer.getDisplayName() + "さん、ようこそ！<br />";
        html += owner.getDisplayName() + "さんのアプリを見ています。";

        document.getElementById("console").innerHTML = html;  
      }

      // ガジェットロード完了後に request() を実行する
      gadgets.util.registerOnLoadHandler(request);
    </script>
    ]]>
    </Content>
  </Module>

このコードでは、OWNERとVIEWERの情報を取り出し、ニックネームを表示するものです。

詳細なプロフィールの取得
------------------------

上のような場合でも、ニックネーム・サムネイルURL・プロフィールURLなどは取得できますが、そのほかのプロフィール項目の取得は手を加える必要があります。オプションを指定することにより、性別や所在地といった情報を取得することができます。


::

  function request() {
    var req = opensocial.newDataRequest();

    var params = {};
    params[opensocial.DataRequest.PeopleRequestFields.PROFILE_DETAILS] = [opensocial.Person.Field.DATE_OF_BIRTH, opensocial.Person.Field.AGE];

    req.add(req.newFetchPersonRequest(opensocial.IdSpec.PersonId.VIEWER, params), "get_viewer");

    req.send(response);
  }

  function response(data) {
    var viewer = data.get("get_viewer").getData();

    var html = viewer.getDisplayName() + "さん（あなた）は"
    + viewer.getField(opensocial.Person.Field.DATE_OF_BIRTH)
    + "生まれで、"
    + viewer.getField(opensocial.Person.Field.AGE)
    + "歳です。";

    document.getElementById("console").innerHTML = html;  
  }

プロフィールのフィールド名は、 opensocial.Person.Field_ を参考にしてください。

OpenPNE3.2-RC1 + opOpenSocialPlugin0.9.3の初期状態では、ニックネーム・サムネイルURL・プロフィールURLに加えて自己紹介・年齢・誕生日・住所（県のみ）を取得の取得が可能です。この項目は、OpenPNEのプリセットプロフィール項目に連動します。

また、もしSNSがサポートしているプロフィール項目を調べたい場合は、 `opensocial.Environment.supportsField()`_ を利用してください。

.. _opensocial.Person.Field: http://wiki.opensocial.org/index.php?title=Opensocial.Person_%28v0.9%29#opensocial.Person.Field
.. _`opensocial.Environment.supportsField()`: http://wiki.opensocial.org/index.php?title=Opensocial.Environment_%28v0.9%29#opensocial.Environment.supportsField

永続データの保存・取得
======================

アプリを所持する人との情報の共有を目的として、永続データを保存・取得することができます。

opOpenSocialPluginでは、以下の制約があります。

* 情報の書き出しは自分のIDでしか行うことができません。
* データを保存するには同じアプリを持っている必要があります。
* 他人の情報を取得するときは、その人がフレンドである必要があります。
