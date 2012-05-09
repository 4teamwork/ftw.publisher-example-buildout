==============================
ftw.publisher example buildout
==============================

This example buildout installs the `ftw.publisher.example` package and demonstrates
how to setup a publishing environment with `ftw.publisher`.

This buildout installs two zope instances, a **instanceSender** and a
**instanceReceiver**. In a productive environment you may wan't to use seperate
buildouts for each instance. The two instances will point to two seperate databases
(var/filestorage/sender.fs and var/filestorage/receiver.fs) respectively.


Setting it up
=============

1. Create a file `buildout.cfg` extending which extends from *plone3.cfg* and
   *sources-readonly.cfg* (optionally).

buildout.cfg:
::
    [buildout]
    extends =
        plone3.cfg
        sources-readonly.cfg

2. Run bootstrap and buildout

shell:
::
    $ python2.4 bootstrap.py
    $ bin/buildout

3. Start the instances

shell:
::
    $ bin/instanceSender start
    $ bin/instanceReceiver start

4. On the **sender** instance (which defaults to port 8080) create a Plone site
   with the id ``sendersite``, on the **receiver** instance (port 9080) create a
   Plone site with the id ``receiversite``.

5. Create a user on the **sender** instance with userid "publisher_cronuser" and the
   password "123456" (according to the clock server configuration in the buildout) and
   give him the Manager role globally.

6. Create a user on the **receiver** instance (useid: "publisher_receiver", password
   "123456") and give him also the Manager role globally.

5. Install the ``ftw.publisher.example`` generic setup profile on both plone sites.

6. On the sender instance, navigate to the publisher control panel
   (http://localhost:8080/sendersite/@@publisher-config) and add a target server,
   using the URL to the receiver plone site (http://localhost:9080/receiversite/) and
   the username "publisher_receiver" with password "123456". Then, test the
   connection - it shoud work.

7. Yay, installation done. Now you can add a new content object and publish it using
   by workflow. It will be automatically added to the publishing queue, which will be
   executed every 10 minutes.

8. If you can't wait 10 minutes, go to the publisher control panel and hit the
   "Execute queue" button ;-) You should get something like that:

expected output:
::
        Executing Queue: 1 of 1 objects to 1 realms
        -
        executing "push" on "Demo" (at /sendersite/demo | UID a3174ad8704d6490af54c3d70985149c)
        ... request data length: 3314
        ... to realm http://localhost:9080/receiversite
        ... got result: ObjectCreatedState
        None

   The object was created, the "None" means that no exceptions were raised at the
   receiver instance, which is good.


=====
Links
=====

The main project package is `ftw.publisher.sender` since it contains all the
configuration panels and the most tools - but without the other mandatory
packages it will not work.
Here are some additional links:

- Publisher packages on pypi: http://pypi.python.org/pypi?%3Aaction=search&term=ftw.publisher&submit=search
- Main github project repository: https://github.com/4teamwork/ftw.publisher.sender
- Issue tracker: https://github.com/4teamwork/ftw.publisher.sender/issues
- Wiki: https://github.com/4teamwork/ftw.publisher.sender/wiki
- Source code repository of the example package: https://github.com/4teamwork/ftw.publisher.example
- Source code repository of the example buildout: https://github.com/4teamwork/ftw.publisher-example-buildout
- Continuous integration: https://jenkins.4teamwork.ch/search?q=ftw.publisher


=========
Copyright
=========

This package is copyright by `4teamwork <http://www.4teamwork.ch/>`_
and licensed under GNU General Public License, version 2.
