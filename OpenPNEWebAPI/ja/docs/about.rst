========================
このドキュメントについて
========================

このドキュメントは、OpenPNE Web API を用い、OpenPNE と情報をやり取りするアプリケーションを作成する方を対象にしています。

OpenPNE Web API は Google Data APIs 1.0 (以下 GData) や RFC 5023 で規定された Atom Publishing Protocol (以下 AtomPub) をベースに作られています。

ただし GData と AtomPub の差異が少なくないことなどを鑑み、このドキュメントにおいては GData もしくは AtomPub と重複する仕様であっても解説していきます。

 * GData の詳細な仕様については http://code.google.com/intl/ja/apis/gdata/docs/1.0/reference.html から参照することができます。
 * AtomPub の詳細な仕様については http://tools.ietf.org/rfc/rfc5023.txt から参照することができます。

表記について
============

例示用の URI
------------

この文書で例示のために用いられる URI は、 URI Template の書式に従って記述されます。

URI Template については http://bitworking.org/projects/URI-Templates/spec/draft-gregorio-uritemplate-03.html を参照してください。

解説のための XPath
------------------

この文書では、例示した XML 文書中の特定要素や属性値などを明確にするために、 XML Path Language 2.0 (以下 XPath) による表記を用います。

XPath の構文などについては http://www.w3.org/TR/xpath20/ を参照してください。

用語
====

用語については Atom Publishing Protocol で定められているものに完全に準拠しますが、定義されている用語がすべて本文書で使われているわけではないことに注意してください。

