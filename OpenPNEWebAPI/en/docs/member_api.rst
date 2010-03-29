==============
API for Member
==============

Available Features
==================

.. toctree::
   :maxdepth: 1

   member_api/get_feed
   member_api/get_resource

Entry Point
===========

Collection URI
    http://example.com/api.php/feeds/member

Member URI
    http://example.com/api.php/feeds/member/{ID of the member}

Format of Entry Document
========================

An entry for a member is described like the following Atom Entry Document:

::

  <?xml version="1.0" encoding="UTF-8" ?>
  <entry xmlns="http://www.w3.org/2005/Atom">
    <id>http://example.com/member/{member_id}</id>
    <published>2009-01-23T16:21:12+09:00</published>
    <updated>2009-01-23T16:21:12+09:00</updated>
    <title type="text">Nickname</title>
    <content type="xhtml">
      <div xmlns="http://www.w3.org/1999/xhtml">
        <div id="sex">Sex</div>
        <div id="birthday">Birthday</div>
        <div id="self_intro">Self Introduction</div>
      </div>
    </content>
    <author>
      <email>example@example.com</email>
    </author>
    <link rel="self" type="application/atom+xml" href="http://example.com/api.php/feeds/member/{member_id}"/>
    <link rel="alternate" type="text/html" href="http://example.com/member/{member_id}"/>
    <link rel="alternate" href="http://example.com/mobile_frontend.php/member/{member_id}"/>
    <link rel="enclosure" href="URL for Profile Image" />
  </entry>

The following is explaining for items in this entry.

 * /entry/id : URN as ID for identifying the entry (optional)
 * /entry/published : Created date of the entry (optonal)
 * /entry/updated : Updated date of the entry (optional)
 * /entry/title : Nickname of the entry
 * /entry/content : Profiles of the entry
    * /entry/content/div/div[@id="Identifier of Profile"] : A value of the profile
 * /entry/author : Informations for this member of the entry
    * /entry/author/email : An E-mail Address of this member
 * /entry/link[@rel="self"]/@href : URI for accessing via OpenPNE Web API (optional)
 * /entry/link[@rel="alternate" and @type="text/html"]/@href : URI for accessing via PC (optional)
 * /entry/link[@rel="alternate" and not(@type)]/@href : URI for accessing via mobile (optional)
 * /entry/link[@rel="enclosure"]/@href : An image of the entry (optional)

