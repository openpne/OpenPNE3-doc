The way to release OpenPNE3
===========================

About
-----

This is a procedure manual for releasing OpenPNE3.

Create the bandled plugins list
-------------------------------

Check out the bandled-plugins-list directory::

  $ svn co https://trac.openpne.jp/svn/OpenPNE3/bandled-plugins-list/

Create the list for this version of OpenPNE::

  $ svn copy bandled-plugins-list/$VERSION-dev.yml bandled-plugins-list/$VERSION.yml
  $ vi bandled-plugins-list/$VERSION.yml
  $ svn ci -m "added the bandled plugins list for ${VERSION}" bandled-plugins-list
  $ svn copy bandled-plugins-list/$VERSION.yml bandled-plugins-list/$NEXT_VERSION-dev.yml
  $ svn ci -m "added the bandled plugins list for development of ${NEXT_VERSION}" bandled-plugins-list

Update the list in the plugin channel server::

  $ ssh $PLUGIN_CHANNEL_SERVER_HOST_NAME
  $ cd /path/to/web/directory
  $ svn up packages

Updated version definition file
--------------------------------

Modify the data/version.php::

  $ git clone git@github.com:openpne/OpenPNE3.git
  $ cd OpenPNE3/
  $ git checkout -b localBranch ${trunk-or-stable-branch}
  $ perl -pi -e 's/\-dev//g' data/version.php
  $ git add data/version.php
  $ git commit -m "version ${VERSION}"
  $ git push

Tagging
-------

Execute the following command::

  $ git tag "${VERSION}"
  $ git push --tags

Updated version definition file to develop
------------------------------------------

Modify the data/version.php::

  $ perl -pi -e 's/${VERSION}/${NEXT_VERSION}-dev/g' data/version.php
  $ git add data/version.php
  $ git commit -m "started developing OpenPNE ${NEXT_VERSION}"
  $ git push
