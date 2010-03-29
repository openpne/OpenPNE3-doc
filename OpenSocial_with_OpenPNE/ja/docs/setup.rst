============
セットアップ
============

OpenPNE3にopOpenSocialPluginを導入することで利用できます。ただし、そのまま本番環境での利用は問題があります。なぜならば、初期の状態だと、JavaScriptのコードを含むガジェットアプリケーションを表示するコンテナが同一ドメイン上で実行されるため、セキュリティ上の問題が起きることが考えられるからです。よって、本番運用では外部ドメイン上にコンテナサーバを設置する必要があるのです。

そこで、いくつかの設定により、コンテナサーバを別に用意することができます。

その場合、SNSサーバ・コンテナサーバは共に、PHPのmcrypt拡張, curl拡張, openssl拡張を導入している必要があります。

コンテナサーバを用意する
========================

opOpenSocialPluginで、外部コンテナを利用する場合はShindig-1.1-BETA3以上を必要としています。

Shindig-1.1-BETA3を利用する場合、 http://svn.apache.org/repos/asf/incubator/shindig/tags/shindig-project-1.1-BETA3-incubating/ から、Shindig をチェックアウトし、 http://incubator.apache.org/shindig/developers/php/build.html に従い導入してください。

OpenPNEの設定
=============

OAuth設定
---------

**http://sns.example.com/pc_backend.php/connection** にアクセスして、アプリケーション登録から、アプリケーションを登録してください。

使用するAPIには、*OpenSocial API* を含めるようにしてください。

登録後に Consumer key と Consumer secret が表示されます。これは、後ほど利用します。


openpne.js
----------

OpenPNEの管理画面から、プラグイン設定 > opOpenSocialPluginの設定 > コンテナ設定 にアクセスします。

「外部のOpenSocialコンテナを利用(推奨)」にチェックを入れ、OpenSocialコンテナURLに、上記で用意したコンテナのURLを設定します。

設定変更後、「Download openpne.js」が表示されます。そのファイルをダウンロードし、 Shindigのディレクトリの *config/* 内に設置してください。

このファイルは、利用できるプロフィール項目の情報を含むためプロフィールを追加、削除した場合は再度上書きするようにしてください。

OpenPNEService.php
------------------

OpenPNEのディレクトリ中の、 *plugins/opOpenSocialPlugin/lib_for_shindig/OpenPNEService.php.sample* を、 *OpenPNEService.php* という名前でコピーしてShindigがロードできる場所に設置してください。

また、ファイルを開き以下の箇所を設定してください。::

  const SNSURL          = '#SNS_URL#';

例 ::

  const SNSURL          = 'http://sns.example.com/';

*#SNS_URL#* はSNS自体のURLを最後はスラッシュで終わるように設定してください。


Shindigの設定
=============

Shindigのディレクトリの *php/config/* に、 *local.php* を作成し、以下のように設定してください。

例::

  <?php

  $shindigConfig = array(
    'debug' => false,
    'token_cipher_key' => '#TOKEN_CIPHER_KEY#',
    'token_hmac_key' => '#TOKEN_HMAC_KEY#',
    'extension_class_paths' => '#EXTENSION_CLASS_PATH#',
    'extension_autoloader' => true,
    'person_service'   => 'OpenPNEService',
    'activity_service' => 'OpenPNEService',
    'app_data_service' => 'OpenPNEService',
    'messages_service' => 'OpenPNEService',
    'album_service' => 'OpenPNEService',
    'media_item_service' => 'OpenPNEService',
  );

*#TOKEN_CIPHER_KEY#* と、 *#TOKEN_HMAC_KEY#* には、コンテナ設定画面にある、「トークン暗号化キー」と「トークンハッシュ化キー」をそれぞれ設定してください。 *#EXTENSION_CLASS_PATH#* には、上記で作成した、 *OpenPNEService.php* のあるディレクトリを指定してください。
