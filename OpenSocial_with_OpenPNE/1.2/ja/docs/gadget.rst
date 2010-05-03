.. _gadget:

======
Gadget
======

SNS上で動作するOpenSocialを利用したアプリは、アプリ名などのメタデータとコンテンツ情報 (JavaScriptやHTML、URLなど) を記述した Gadget XML ファイルを作成し、SNSサーバ、コンテナサーバに読み込ませることにより実現します。

Gadget XMLの仕様は `OpenSocial Gadgets API Specification v0.9`_ を基にしています。

.. _`OpenSocial Gadgets API Specification v0.9`: http://www.opensocial.org/Technical-Resources/opensocial-spec-v09/Gadgets-API-Specification.html

基本構造とアプリの追加
======================

もっとも単純なGadget XMLは以下のようになります。

なお、このアプリはPC限定になります。

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


設定時可能なアプリ情報について
------------------------------

ModulePrefsタグの属性は以下を利用することができます。

title (必須)
  アプリ名
title_url
  タイトルURL
description
  アプリ詳細
author
  作成者
author_email
  作成者のメールドレス
thumbnail
  サムネイルURL
author_affiliation
  作成者の所属
author_photo
  作成者の写真URL
author_aboutme
  作成者について
author_link
  作成者リンク
height
  コンテンツの高さ(ピクセル)
scrolling
  スクロールバーの表示

ライフサイクルイベント
----------------------

ModulePrefsタグにLinkタグを含ませることにより、ライフサイクルイベントを利用することができます。

詳細は、 :ref:`こちら<lifecycle_event>` を御覧下さい。
