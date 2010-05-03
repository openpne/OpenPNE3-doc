==============
JavaScript API
==============

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
