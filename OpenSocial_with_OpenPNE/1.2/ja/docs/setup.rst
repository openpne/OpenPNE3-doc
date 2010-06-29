.. _setup:

============
セットアップ
============

この章では、SNSおよびOpenSocialコンテナ設置者向けのの情報を提供します。

opOpenSocialPlugin を正常に動作させるには、OpenPNE3が必要な環境に加え以下の環境がSNSサーバに備えられている必要があります。

 * PHPのopenssl拡張

署名用の鍵を作成する
====================

SNSが、アプリケーション開発者のサーバなどにHTTPリクエストを送信する際は、:ref:`署名<signature>` (OAuth Signature)をリクエストパラメータやHTTPヘッダに付加します。その際に、RSA秘密鍵とCSRが必要になるため、事前に作成する必要があります。

以下の手順で作成することが出来ます。

OpenPNEのディレクトリのトップに移動し、以下のタスクを実行します。

::

  $ php symfony opOpenSocial:generate-key

秘密鍵とCSRの作成に必要な情報（有効期限、キーフレーズ、国コード、所在地など）等を入力します。入力が終わると、 *plugins/opOpenSocialPlugin/certs* 下に鍵が作成されます。また、 http://sns.example.com/opensocial/certificates でCSRを見ることが出来ます。

署名の検証法は こちら を御覧下さい。


定期的にライフサイクルイベントを送信するようにする
==================================================

opOpenSocialPlugin ではライフサイクルイベントによるリクエストに対応していますが、特定のイベントが発生した時にはリクエストは送信されません。特定のイベントが発生したときはキューに蓄積され、特定のタスクが実行されたときに一度にリクエストします。

よってSNS設置者は以下を、cronなどで定期的に実行する必要があります。

::

  cd #OPENPNE_DIR# && php symfony opOpenSocial:execute-lifecycle-event

**#OPENPNE_DIR#** はOpenPNEのディレクトリを指定して下さい。

オプションとして、1回の実行ごとのリクエスト数を制限することが出来ます。 --limit-request で数値を指定すると、リクエスト数の上限を設定することができます。また、 --limit-request-app に数値を指定することにより、アプリごとのリクエスト数の上限を設定することができます。負荷の状況などにあわせて設定して下さい。

コンテナサーバーを外部にする
============================

ガジェットアプリやAPIはOpenPNE3にopOpenSocialPluginを導入することで利用できます。ただし、そのまま本番環境での利用は問題があります。なぜならば、初期の状態では、JavaScriptのコードを含むガジェットアプリケーションを表示するコンテナが同一ドメイン上で実行されるため、セキュリティ上の問題が起きる可能性が生じます。よって、本番運用では外部ドメイン上にコンテナサーバを設置する必要があるのです。

そこで、いくつかの設定により、コンテナサーバを別に用意することができます。

その場合、SNSサーバ・コンテナサーバは共に、PHPのmcrypt拡張, curl拡張, openssl拡張を導入している必要があります。

opOpenSocialPluginで、対応しているコンテナはShindig-1.1-BETA5以上を必要としています。

Shindig-1.1-BETA5を利用する場合、Subversionにて http://svn.apache.org/repos/asf/shindig/tags/shindig-project-1.1-BETA5-incubating/ から、Shindig をチェックアウトし、 http://incubator.apache.org/shindig/developers/php/build.html に従い導入してください。

また、上記で作成した鍵はShindigのディレクトリの *php/certs/* へコピーして下さい。

OpenPNEの設定
-------------

openpne.js
~~~~~~~~~~

OpenPNEの管理画面から、プラグイン設定 > opOpenSocialPluginの設定 > コンテナ設定 にアクセスします。

「外部のOpenSocialコンテナを利用(推奨)」にチェックを入れ、OpenSocialコンテナURLに、上記で用意したコンテナのURLを設定します。

設定変更後、「Download openpne.js」が表示されます。そのファイルをダウンロードし、 Shindigのディレクトリの *config/* 内に設置してください。

このファイルは、利用できるプロフィール項目の情報を含むためプロフィールを追加、削除した場合は再度上書きするようにしてください。

OpenPNEService.php
~~~~~~~~~~~~~~~~~~

OpenPNEのディレクトリ中の、 *plugins/opOpenSocialPlugin/lib_for_shindig/OpenPNEService.php.sample* を、 *OpenPNEService.php* という名前でコピーしてShindigがロードできる場所に設置してください。

また、ファイルを開き以下の箇所を設定してください。::

  const SNSURL          = '#SNS_URL#';

例 ::

  const SNSURL          = 'http://sns.example.com/';

*#SNS_URL#* はSNS自体のURLを最後はスラッシュで終わるように設定してください。


Shindigの設定
-------------

Shindigのディレクトリの *php/config/* に、 *local.php* を作成し、以下のように設定してください。

例::

  <?php

  $shindigConfig = array(
    'debug' => false,

    'allow_plaintext_token' => false,
    'render_token_required' => true,
    'allow_anonymous_token' => false,

    'token_cipher_key' => '#TOKEN_CIPHER_KEY#',
    'token_hmac_key'   => '#TOKEN_HMAC_KEY#',
    'private_key_phrase'    => '#KEY_PHRASE#',
    'extension_class_paths' => '#EXTENSION_CLASS_PATH#',
    'extension_autoloader'  => true,
    'person_service'     => 'OpenPNEService',
    'activity_service'   => 'OpenPNEService',
    'app_data_service'   => 'OpenPNEService',
    'messages_service'   => 'OpenPNEService',
    'album_service'      => 'OpenPNEService',
    'media_item_service' => 'OpenPNEService',
  );

*#TOKEN_CIPHER_KEY#* と、 *#TOKEN_HMAC_KEY#* には、コンテナ設定画面にある、「トークン暗号化キー」と「トークンハッシュ化キー」をそれぞれ設定してください。 *#KEY_PHRASE#* には、秘密鍵のキーフレーズを指定し、 *#EXTENSION_CLASS_PATH#* には、上記で作成した、 *OpenPNEService.php* のあるディレクトリを指定してください。
