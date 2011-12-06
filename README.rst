======================================
anyremote-j2me-client
======================================

This is a custom build that I made for the j2me client that is distributed with
`anyremote <http://anyremote.sourceforge.net/>`_. This is not a fork. It's
more like a re-packaging. 

I repackaged the build to use reuse an existing j2me ant + antenna build that I
had instead of the make+autotools system currently used by the upstream
sourceforge project. I did this because my template j2me ant+antenna build
system already had support for signing MIDlets with an x509 certificate in a
java keystore.  I need this because the j2me handset that I use with anyremote
does not allow unsigned MIDlets to use the jsr82 bluetooth APIs.

If you are using a nokia phone and want to install your own code signing certificate
on the device, take a look at one of these projects:

    - `dustbowl <https://github.com/gorlowski/dustbowl>`_
    - `nokicert <http://code.google.com/p/nokicert/>`_

This is currently tracking the *2010-04-21* version of anyremote-j2me-client.

-------------------
Build Documentation
-------------------

~~~~~~~~~~~~~~~~~~~~~~~~
Build Variant Properties
~~~~~~~~~~~~~~~~~~~~~~~~

icon.size: 
        set the icon size used in the output jar. The default "all", will
        include all available icon set sizes

jsr82: 
        set this to false to create a nojsr82 build (defaults to true)

sign.app: 
        set to true to sign the jad file with an x509 certificate (defaults to
        false, which means the jad will not be signed)

        NOTE: 
                you can create a src/build/user.properties file or set the
                user.properties.location to the path of an external properties
                file that can override properties used in the build.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Signing your MIDlet jad file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To sign the midlet, you must set sign.app=true when you build.

You must also set these properties:

        keystore.file: 
                        the location of a java keystore with your cert + keys

        storepass: 
                        the password of your keystore

        certalias: 
                        the alias of your cert+private key pair in your
                        keystore

        certpass: 
                        the password of the private key associated with the
                        [certalias] cert in your keystore

~~~~~~~~~
Examples
~~~~~~~~~

Build a variant with all icon sizes + jsr82 enabled. Do not sign the jad.::

          ant package

Build a variant with jsr82 enabled and 64px icons, and sign the MIDlet (by
default, the jar will not be signed)::

          ant -Dicon.size=64 -Djsr82=true -Dsign.app=true package

Clean and then build a non-jsr82 variant, using 48px icons. Do not sign the
jad. *NOTE*: jsr82 defaults to true::

          ant -Dicon.size=48 -Djsr82=false clean package

Build, package and then run the MIDlet.::

        ant run

~~~~~~~~~~~
Other Notes
~~~~~~~~~~~

If you do not want to use the ant maven plugin to download the build deps
(proguard and antenna), then you can explicitly set the location of
the jars on your filesystem::

        ant -Dwtk.proguard.home=/path/to/your/proguard/install/dir -Dantenna.jar=/path/to/your/antenna.jar package

