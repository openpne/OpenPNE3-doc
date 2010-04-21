====
署名
====

OpenPNE側からアプリ開発者宛に発行される様々なリクエストには署名が付加される場合があります。これは、悪意のある第三者がアプリ開発者宛に直接リクエストを行った場合にはアプリが異常な動作を起こすことがあります。そのため、署名のついたリクエストを受信した際は必ず署名が、正しいかを検証する必要があります。

署名方式として、「OAuth Core 1.0 Rev A」のRSA-SHA1を利用します。

公開鍵
------

公開鍵は、 http://sns.example.com/opensocial/certificates で確認ができます。有効期限はSNS設置者が公開する情報に従って下さい。

リクエストの検証
----------------

署名の方式については、 http://oauth.net/core/1.0a/#signing_process の仕様に従います。

また、署名の検証に必要なパラメータはリクエストのパラメータか、Authorizationヘッダに付加されます。

サンプルコード(PHP)
-------------------

このコードは、ライブラリとしてAndy SmithのOAuthライブラリ http://oauth.googlecode.com/svn/code/php/ のr1188 で動作検証を行っています。

署名の検証に失敗すると、"invalid"が表示されスクリプトが停止します。実際には、適切なレスポンスコードを返す処理などを行う必要があります。

*#CERTIFICATE#* には入手した公開鍵の内容に置き換えて下さい。

例::

  include_once 'lib/OAuth.php';

  class OAuthSignatureMethod_RSA_SHA1_opOpenSocialPlugin extends OAuthSignatureMethod_RSA_SHA1
  {
    protected function fetch_private_cert(&$request) {
    }

    protected function fetch_public_cert(&$request) {
      return <<<EOT
  -----BEGIN CERTIFICATE-----
  #CERTIFICATE#
  -----END CERTIFICATE-----
  EOT;
    }
  }

  $request = OAuthRequest::from_request(null, null, null);
  $signature_method = new OAuthSignatureMethod_RSA_SHA1_opOpenSocialPlugin();
  if (!$signature_method->check_signature($request, null, null, $request->get_parameter('oauth_signature')))
  {
    echo "invalid";
    exit;
  }

  ...
