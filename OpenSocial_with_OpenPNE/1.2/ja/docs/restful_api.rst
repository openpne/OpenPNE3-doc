.. _restful_api:

===========
RESTful API
===========

マッシュアップサイト作成や、モバイルアプリ用のリソースアクセスの手段として `OpenSocial RESTful API`_ を利用するという手段があるでしょう。

この章ではOpenSocial RESTful APIについて解説します。

.. _`OpenSocial RESTful API`: http://www.opensocial.org/Technical-Resources/opensocial-spec-v09/REST-API.html

OAuth
=====

APIへのアクセス認可にはOAuthを利用します。

OpenPNE3 + opOpenSocialPlugin でのOAuthコンシューマは以下の3種類が挙げられます。

* 利用者側で登録されたコンシューマ
* 管理者画面側で登録されたコンシューマ
* アプリ提供者が登録したコンシューマ (認可手段は2-legged OAuthのみ)

管理者側の認可によって発行されたアクセストークンを利用する場合は、どの利用者の情報を取得するのかを明示する必要があります。そのため、最終的にAPIにアクセスするときに *xoauth_requestor_id* でメンバーIDを指定する必要があります。

また、 `2-legged OAuth`_ を利用することができますが、利用者側で登録されたコンシューマであった場合、 *xoauth_requestor_id* は、その利用者自身のメンバーIDでなければいけません。

OpenPNE上のOAuth認可については、 http://sandbox.ebihara.dazai.pne.jp/oauth.ja.html を参照してください。

.. _`2-legged OAuth`: http://oauth.googlecode.com/svn/spec/ext/consumer_request/1.0/drafts/1/spec.html


メンバー情報の取得
==================

エントリポイント
----------------

  GET http://sns.example.com/api.php/social/rest/people/\ *userId*\ /\ *selector*

例
--

GET http://sns.example.com/api.php/social/rest/people/@me/@self ::

  {
    "entry": {
    "isOwner":true,
    "isViewer":true,
    "id":"1",
    "displayName": "OpenPNE\u541b",
    "thumbnailUrl":"http:\/\/sns.example.com\/cache\/img\/jpg\/w_h\/m_1_70481ff86b46bdb146bc78781a4c2a2d9a1581f6_jpg.jpg",
    "profileUrl":"http:\/\/sns.example.com\/member\/1"
    }
  }

*@me* は *userId* に自分のIDを指定した場合と同様です。 *@self* は *userId* で指定したメンバー自身の情報ということになります。 *selector* を *@friend* とすると指定メンバーのフレンド情報を取得します。

パラメータとして、何も指定しない場合は、JSON形式でデータが返されます。

GET http://sns.example.com/api.php/social/rest/people/@me/@self?format=xml ::

  <?xml version="1.0" encoding="UTF-8"?>
  <response>
    <entry>
      <id>1</id>
      <isOwner>1</isOwner>
      <isViewer>1</isViewer>
      <displayName>OpenPNE君</displayName>
      <thumbnailUrl>http://sns.example.com/cache/img/jpg/w_h/m_1_70481ff86b46bdb146bc78781a4c2a2d9a1581f6_jpg.jpg</thumbnailUrl>
      <profileUrl>http://sns.example.com/member/1</profileUrl>
    </entry>
  </response>

パラメータとして format を xml に指定することにより、XML形式でのデータの取得も可能です。ほかにも、atom を指定することにより ATOM形式によるデータの取得ができます。
これ以降で紹介する、レスポンスのあるリクエストでは同様にformatを指定することができます。

GET http://sns.example.com/api.php/social/rest/people/@me/@self?fields=birthday,age,languagesSpoken ::

  {
    "entry": {
      "id":"1",
      "isOwner":true,
      "isViewer":true,
      "displayName":"OpenPNE\u541b",
      "thumbnailUrl":"http:\/\/sns.example.com\/cache\/img\/jpg\/w_h\/m_1_70481ff86b46bdb146bc78781a4c2a2d9a1581f6_jpg.jpg",
      "profileUrl":"http:\/\/sns.example.com\/member\/1"
      "age":21,
      "birthday":"1988\/04\/23",
      "languagesSpoken":"ja"
    }
  }

パラメータとして fields に、コンマ区切りで取得したいプロフィール項目を指定することにより、そのプロフィール情報も取得できます。OpenPNE3.4<= + opOpenSocialPlugin1.2 の初期状態では、 aboutMe, addresses, age, birthday, languagesSpoken, gender といった項目に対応されています。

また、結果が複数件存在する場合は以下のようになります。(@meのフレンドを取得します。フレンドがAとBの2人存在する場合です。)

GET http://sns.example.com/api.php/social/rest/people/@me/@friend ::

  {
    "entry": [
      {
        "id":"2",
        "isOwner":false,
        "isViewer":false,
        "displayName":"A",
        "thumbnailUrl":"",
        "profileUrl":"http:\/\/sns.example.com\/member\/2"
      },
      {
        "id":"3",
        "isOwner":false,
        "isViewer":false,
        "displayName":"B",
        "thumbnailUrl":"",
        "profileUrl":"http:\/\/sns.example.com\/member\/3"
      }
    ],
    "startIndex":0,
    "totalResults":2,
    "itemsPerPage":20
  }

このように結果が、リストになります。デフォルトでは一度に２０件のデータが取得可能です。開始インデックスは、パラメータとしてstartIndexに数値を指定することにより変更が可能です。

アプリ所有者限定の一覧を取得したい場合は、filterByパラメータにhasAppを指定して下さい。(**アプリごとに発行したコンシューマキーを利用してAPIアクセスを必要があります。**)

アクティビティ
==============

アプリの活動状況等を共有する仕組みとしてアクティビティが存在します。


エントリポイント
----------------

アクティビティの投稿

  POST http://sns.example.com/api.php/social/rest/activities/@me/@self

アクティビティの取得

  GET http://sns.example.com/api.php/social/rest/activities/\ *userId*\ / *selector* \/ *appId*


例
--

アクティビティの投稿は、
POST http://sns.example.com/api.php/social/rest/activities/@me/@self で以下のような内容を送信することにより行うことができます。
このとき Content-Type は application/json として下さい。

::

  {
    "title": "hello!",
    "url": "http://sns.example.com/..."
  }

「hello!」 という内容のActivityが送信されます。APIでのアクティビティの投稿間隔には制限があります。デフォルトでは30秒以内の間隔で投稿することはできません。この秒数はSNSの管理画面より変更することができます。
この制限により、投稿が失敗した場合はレスポンスコード500のエラーを返します。

アクティビティの公開範囲は、利用者が設定したアプリの公開範囲に準じます。

オプションとして、アクティビティにURL情報を付加することができますが、そのURLはSNSのドメインと同一である必要があります。

アクティビティの取得は以下のように行います

GET http://sns.example.com/api.php/social/rest/activities/@me/@self::

  {
    "entry": [
      {
        "id":"2",
        "userId":"1",
        "title":"hogehoge",
        "postedTime":"2010-04-21T21:02:56+09:00"
      },
      {
        "id":"1",
        "userId":"1",
        "title":"foobar",
        "postedTime":"2010-04-21T19:09:19+09:00"
      }
    ],
    "startIndex":0,
    "totalResults":2,
    "itemsPerPage":20
  }

この状態では、アプリを指定していないので、発行元の関係なく指定のメンバーのアクティビティストリームを取得ができます。

**アプリごとに発行しているコンシューマキーを利用してアクセスしている場合** は以下が利用できます。

GET http://sns.example.com/api.php/social/rest/activities/@me/@self/@app::

  {
    "entry": [
      {
        "id":"2",
        "userId":"1",
        "title":"hogehoge",
        "postedTime":"2010-04-21T21:02:56+09:00"
      },
    ],
    "startIndex":0,
    "totalResults":1,
    "itemsPerPage":20
  }

これにより、アプリが発行したアクティビティのみを表示することができます。

永続データ
==========

永続データはアプリを所有する人同士での情報の共有などで利用することの出来る機能です。

アプリ・メンバーごとにKey-Valueの組み合わせで情報を管理します。

情報の書き出し、削除は自分のIDにしか行うことができません。取得は、対象者が取得者のフレンドかつアプリ所有をしていた場合に行うことができます。

**この機能は、アプリごとに発行したコンシューマキーを利用してAPIアクセスをする必要があります。**

エントリポイント
----------------

永続データの作成・更新

  POST http://sns.example.com/api.php/social/rest/appdata/@me/@self/@app

永続データの取得

  GET http://sns.example.com/api.php/social/rest/appdata/\ *userId*\ / *selecter* \/@app

永続データの削除

  DELETE http://sns.example.com/api.php/social/rest/appdata/@me/@self/@app

例
--

永続データの作成は
POST http://sns.example.com/api.php/social/rest/appdata/@me/@self/@app で以下のような内容を送信することにより行うことができます。
このとき Content-Type は application/json として下さい。

::

  {
    "foo1":"bar1",
    "foo2":"bar2",
    "foo3":"bar3"
  }

foo1=bar1、foo2=bar2が保存されます。すでに、当該キーが存在する場合は上書きされます。

取得は以下のように行えます。

GET http://sns.example.com/api.php/social/rest/appdata/@me/@self/@app::

  {
    "entry": {
      "1": {
        "foo1":"bar1",
        "foo2":"bar2",
        "foo3":"boo3"
      }
    }
  }

他人の永続データを取得する場合は、その人がVIEWERのフレンドであり、かつアプリを所有している必要があります。

削除は

DELETE http://sns.example.com/api.php/social/rest/appdata/@me/@self/@app?fields=foo1,foo2

のようにfieldsパラメータにカンマ区切りでキーを指定するにより削除を行うことができます。

fieldsパラメータが存在しない場合は、そのメンバーのアプリについての永続データが全て削除されます。


アルバム情報の取得
==================

opOpenSocialPluginでは、opAlbumPluginと連動してアルバムの情報を取得することができます。opAlbumPluginが導入されていない場合はこの機能は利用できません。

エントリポイント
----------------

アルバム情報の取得

  GET http://sns.example.com/api.php/social/rest/albums/\ *userId*\ /\ *selector*

アルバム内容の取得

  GET http://sns.example.com/api.php/social/rest/mediaitems/\ *userId*\ /\ *selector*\ /\ *albumId*

例
--

GET http://sns.example.com/api.php/social/rest/albums/@me/@self ::

  {
    "entry": [
      {
        "id":"1",
        "title":"album title",
        "description":"foo",
        "mediaItemCount":2,
        "ownerId":"1",
        "thumbnailUrl":"http:\/\/sns.example.com\/cache\/img\/jpg\/w180_h180\/d906f3049dfc809473603132dade9b8484a31887_gif.jpg",
        "mediaType":"IMAGE"
      }
    ],
    "startIndex":0,
    "totalResults":1,
    "itemsPerPage":20
  }

アルバム自体の情報の取得が可能です。

アルバムの内容を取得したい場合は、以下のようにします。

GET http://sns.example.com/api.php/social/rest/mediaitems/@me/@self/1 ::

  {
    "entry":  [
      {
        "albumId":"1",
        "created":"2009-11-30 22:57:00",
        "description":"foo",
        "fileSize":"0",
        "id":"1",
        "lastUpdated":"2009-11-30 22:57:00",
        "thumbnailUrl":"http:\/\/sns.example.com\/cache\/img\/jpg\/w180_h180\/a_1_7b0e61f64a2ee2ef183b05f1c9d8161f251d139a_jpg.jpg",
        "title":"title",
        "type":"IMAGE",
        "url":"http:\/\/sns.example.com\/cache\/img\/jpg\/w_h\/a_1_7b0e61f64a2ee2ef183b05f1c9d8161f251d139a_jpg.jpg"
      },
      {
        "albumId":"1",
        "created":"2009-11-30 22:57:00",
        "description":"bar",
        "fileSize":"0",
        "id":"2",
        "lastUpdated":"2009-11-30 22:57:00",
        "thumbnailUrl":"http:\/\/sns.example.com\/cache\/img\/jpg\/w180_h180\/a_1_7b0e61f64a2ee2ef183b05f1c9d8161f251d139a_jpg.jpg",
        "title":"title",
        "type":"IMAGE",
        "url":"http:\/\/sns.example.com\/cache\/img\/jpg\/w_h\/a_1_7b0e61f64a2ee2ef183b05f1c9d8161f251d139a_jpg.jpg"
      }
    ],
    "startIndex":0,
    "totalResults":1,
    "itemsPerPage":20
  }
