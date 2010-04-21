========================
このドキュメントについて
========================

このドキュメントは OpenPNE と opOpenSocialPlugin を利用した OpenSocialアプリケーション開発についての情報を提供するものです。

OpenSocialについて
==================

OpenSocialは、SNSのリソースにアクセスするための共通なAPI群です。SNSメンバーのリソースにアクセスするためのAPIが規定されており、SNSのガジェットやマッシュアップサイトなどで利用することができます。

もし OpenSocial に関する詳細な情報を入手したければ、 http://code.google.com/apis/opensocial を訪れてください。


opOpenSocialPluginについて
==========================

opOpenSocialPluginはOpenPNE3上で、OpenSocialのAPI、ガジェットをサポートするためのプラグインです。

opOpenSocialPlugin0.9.3 の段階では以下の機能をサポートします。

* 利用可能API
    * Person API
    * Persistence API
    * Albums API
    * MediaItems API
* ガジェット機能

今後、Activity APIにも対応される予定です。

開発は http://github.com/kawahara/opOpenSocialPlugin で行われています。fork, pull request 大歓迎です。

表記について
============

* このドキュメントでは、OpenPNE3のSNSが **http://sns.example.com/** でアクセスできるようにインストールされていると仮定しています。
