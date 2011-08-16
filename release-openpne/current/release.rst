The way to release OpenPNE3
===========================

About
-----

This is a procedure manual for releasing OpenPNE3.

Create the bundled plugins list
-------------------------------

Check out the bundled-plugins-list directory::

  $ git clone git@github.com:openpne/bundled-plugins-list.git

Create the list for this version of OpenPNE::

  $ cd bundled-plugins-list
  $ cp $VERSION-dev.yml $VERSION.yml
  $ vi $VERSION.yml
  $ git add $VERSION.yml
  $ git commit -m "added the bundled plugins list for ${VERSION}"
  $ cp $VERSION.yml $NEXT_VERSION-dev.yml
  $ git add $NEXT_VERSION-dev.yml
  $ git commit -m "added the bundled plugins list for development of ${NEXT_VERSION}"
  $ git push origin master

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
