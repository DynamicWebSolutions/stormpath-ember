.. _upgrading:


Upgrade Guide
=============

This page contains specific upgrading instructions to help you migrate between
Express-Stormpath releases.

Version 2.2.0 -> Version 2.3.0
--------------------------------

**Registration changes may be needed**

If you are supplying extra fields on registration (fields that are added to the
user's custom data object), and you are using an HTML-based form submission, you
now need to declare these fields in the registration configuration.

This change is necessary because we fixed this library to *not* allow arbitrary
submission of data to the account's custom data object.

Please see the registration section of this documentation for examples of how to
declare your custom fields.

The default value for first name and last name is now "UNKNOWN", if not provided
and not required during registration.  Previously it was "Anonymous".  If you
are depending on this value you will need to change your test to look for
"UNKONWN".

Version 2.1.0 -> Version 2.2.0
--------------------------------

**Changes may be needed**

The Oauth2 endpoint is now enabled by default.  You no longer need to use this
configuration option is you want to enable the OAuth2 endpoint:

.. code-block:: javascript

    stormpath.init(app, {
      api: true
    });

If you wish to *disable* the Oauth2 endpoint you can do so like this:

.. code-block:: javascript

    stormpath.init(app, {
      web: {
        oauth2: {
          enabled: false
        }
      }
    });


Version 2.0.14 -> Version 2.1.0
--------------------------------

**No changes needed!**


Version 2.0.13 -> Version 2.0.14
--------------------------------

**No changes needed!**


Version 2.0.12 -> Version 2.0.13
--------------------------------

**No changes needed!**


Version 2.0.9 -> Version 2.0.10
-------------------------------

We were looking for the option ``config.web.register.autoAuthorize``, to
enable the auto-login-after-registration feature.   This should actually be
``autoLogin``, and it is documented as this. The library is now looking for
this option as ``autoLogin``, so you will need to change your configuration it
if you were using ``autoAuthorize``.


Version 2.0.8 -> Version 2.0.9
------------------------------

If you are using the ``/forgot`` endpoint and posting JSON, you need to change
the ``username`` property to ``email``.


Version 2.0.7 -> Version 2.0.8
------------------------------

**No changes needed!**


Version 2.0.6 -> Version 2.0.7
------------------------------

**No changes needed!**


Version 2.0.5 -> Version 2.0.6
------------------------------

**No changes needed!**


Version 2.0.4 -> Version 2.0.5
------------------------------

**No changes needed!**


Version 2.0.3 -> Version 2.0.4
------------------------------

**No changes needed!**


Version 2.0.2 -> Version 2.0.3
------------------------------

**No changes needed!**


Version 2.0.1 -> Version 2.0.2
------------------------------

**No changes needed!**


Version 2.0.0 -> Version 2.0.1
------------------------------

**No changes needed!**


Version 1.0.6 -> Version 2.0.0
------------------------------

**Many changes needed!**

This is a **major release** in the life of this library.  This release includes
tons of new features, refactorings, etc.

To upgrade from **1.0.6**, please pay careful attention to the below notes.

Firstly, this library now takes new configuration options.  When you initialize
the middleware, you'll need to pass in the following basic options:

.. code-block:: javascript

    stormpath.init(app, {
      client: {
        apiKey: {
          id: 'xxx',
          secret: 'yyy'
        }
      },
      application: {
        href: 'https://api.stormpath.com/v1/applications/xxx'
      }
    });

Every setting in the new configuration can also be set via environment
variables.  The way it works is that all nested fields are expanded out to their
full path.  For instance, if you wanted to set `client.apiKey.id`, you could
create an environment variable called::

    STORMPATH_CLIENT_APIKEY_ID=xxx

Likewise, for the rest of the settings above::

    STORMPATH_CLIENT_APIKEY_SECRET=yyy
    STORMPATH_APPLICATION_HREF=https://api.stormpath.com/v1/applications/xxx

Next, we've disabled default login, registration, and logout routes.  To enable
them, you'll want to do the following:

.. code-block:: javascript

    stormpath.init(app, {
      website: true
    });

This will enable the default *website* features this library provides:

- A login page (`/login`).
- A registration page (`/register`).
- A logout route (`/logout`).

Next, we've disabled the `/oauth` endpoint we previously enabled by default.  If
you want to enable this, with its default settings, you can now do the
following:

.. code-block:: javascript

    stormpath.init(app, {
      api: true
    });

Another important thing to note, our old OAuth functionality created a route
that lived at `/oauth`.  When you enable the *new* OAuth endpoint, it will live
at `/oauth/token` instead.  This was done to comply with the OAuth2 spec more
closely, and ensure compatibility between libraries / frameworks.

Other than the above, your upgrade process should go smoothly.  There are, of
course, lots of new features / configuration options, so please read through the
new library documentation to get a feeling for it!

Thanks for reading,

-Randall


Version 1.0.5 -> Version 1.0.6
------------------------------

**No changes needed!**


Version 1.0.4 -> Version 1.0.5
------------------------------

**No changes needed!**


Version 1.0.3 -> Version 1.0.4
------------------------------

If you were previously working with Stormpath sessions directly, then you'll
need to modify your code.  While previously Stormpath sessions were referred to
by ``req.session``, they are now referred to by ``req.stormpathSession``.


Version 1.0.2 -> Version 1.0.3
------------------------------

**No changes needed!**


Version 1.0.1 -> Version 1.0.2
------------------------------

**No changes needed!**


Version 1.0.0 -> Version 1.0.1
------------------------------

**No changes needed!**


Version 0.6.9 -> Version 1.0.0
------------------------------

This is a major release that breaks several things from older releases.

Firstly, if you were previously using the ``postRegistrationHandler`` to perform
custom logic after a new user registers, you'll need to modify this event
handler to accept new arguments.

Previously, the ``postRegistrationHandler`` had a method signature that looked
like this::

    postRegistrationHandler(account, res, next) { ... }

In this release, we're modifying the method signature to look like this::

    postRegistrationHandler(account, req, res, next) { ... }

What we've done is add in a new parameter: ``req``, which is the Express request
object.  This gives you more control over the request, and allows you to do
things like modify session data, etc.

Secondly, we no longer support old sessions.

If you are upgrading directly from an older release (*version 0.2.x*) to this
release, then your existing user sessions will be invalid, and this will force
your users to re-authenticate the next time they visit your site.  This is due
to a change in the way we store session data that was introduced in *version
0.3.x*.

.. note::
    The session change will NOT break your code, but it WILL require your users
    to re-authenticate the next time they visit your site.


Version 0.6.8 -> Version 0.6.9
------------------------------

**No changes needed!**


Version 0.6.7 -> Version 0.6.8
------------------------------

**No changes needed!**


Version 0.6.6 -> Version 0.6.7
------------------------------

**No changes needed!**


Version 0.6.5 -> Version 0.6.6
------------------------------

**No changes needed!**


Version 0.6.4 -> Version 0.6.5
------------------------------

**No changes needed!**


Version 0.6.3 -> Version 0.6.4
------------------------------

**No changes needed!**


Version 0.6.2 -> Version 0.6.3
------------------------------

**No changes needed!**


Version 0.6.1 -> Version 0.6.2
------------------------------

**No changes needed!**


Version 0.6.0 -> Version 0.6.1
------------------------------

If you were previously specifying a value for the
``stormpathIDSiteVerificationFailedView`` setting, you'll need to rename that
field to ``stormpathIdSiteVerificationFailedView``.


Version 0.5.9 -> Version 0.6.0
------------------------------

**No changes needed!**


Version 0.5.8 -> Version 0.5.9
------------------------------

**No changes needed!**


Version 0.5.7 -> Version 0.5.8
------------------------------

**No changes needed!**


Version 0.5.6 -> Version 0.5.7
------------------------------

**No changes needed!**


Version 0.5.5 -> Version 0.5.6
------------------------------

**No changes needed!**


Version 0.5.4 -> Version 0.5.5
------------------------------

**No changes needed!**


Version 0.5.3 -> Version 0.5.4
------------------------------

**No changes needed!**


Version 0.5.2 -> Version 0.5.3
------------------------------

**No changes needed!**


Version 0.5.1 -> Version 0.5.2
------------------------------

**No changes needed!**


Version 0.5.0 -> Version 0.5.1
------------------------------

**No changes needed!**


Version 0.4.9 -> Version 0.5.0
------------------------------

**No changes needed!**


Version 0.4.8 -> Version 0.4.9
------------------------------

**No changes needed!**


Version 0.4.7 -> Version 0.4.8
------------------------------

**No changes needed!**


Version 0.4.6 -> Version 0.4.7
------------------------------

**No changes needed!**


Version 0.4.5 -> Version 0.4.6
------------------------------

**No changes needed!**


Version 0.4.4 -> Version 0.4.5
------------------------------

**No changes needed!**


Version 0.4.3 -> Version 0.4.4
------------------------------

**No changes needed!**


Version 0.4.2 -> Version 0.4.3
------------------------------

- Please upgrade to version 0.4.4 -- this version contains a bug with our user
  middleware which causes permission assertion to always fail.


Version 0.4.1 -> Version 0.4.2
------------------------------

**No changes needed!**


Version 0.4.0 -> Version 0.4.1
------------------------------

**No changes needed!**


Version 0.3.4 -> Version 0.4.0
------------------------------

**No changes needed!**


Version 0.3.3 -> Version 0.3.4
------------------------------

**No changes needed!**


Version 0.3.2 -> Version 0.3.3
------------------------------

**No changes needed!**


Version 0.3.1 -> Version 0.3.2
------------------------------

**No changes needed!**


Version 0.3.0 -> Version 0.3.1
------------------------------

**No changes needed!**


Version 0.2.9 -> Version 0.3.0
------------------------------

**No changes needed!**


Version 0.2.8 -> Version 0.2.9
------------------------------

**No changes needed!**


Version 0.2.7 -> Version 0.2.8
------------------------------

**No changes needed!**


Version 0.2.6 -> Version 0.2.7
------------------------------

**No changes needed!**


Version 0.2.5 -> Version 0.2.6
------------------------------

**No changes needed!**


Version 0.2.4 -> Version 0.2.5
------------------------------

**No changes needed!**


Version 0.2.3 -> Version 0.2.4
------------------------------

**No changes needed!**


Version 0.2.2 -> Version 0.2.3
------------------------------

**No changes needed!**


Version 0.2.1 -> Version 0.2.2
------------------------------

**No changes needed!**


Version 0.2.0 -> Version 0.2.1
------------------------------

**No changes needed!**


Version 0.1.9 -> Version 0.2.0
------------------------------

If you were previously relying on the built-in CSRF validation in your pages,
you'll need to include CSRF manually.  This release no longer includes CSRF
token protection on *all* pages -- it only protects the Stormpath pages --
this was done to be less confusing for users.

To add CSRF protection to your site similar to what was included automatically
before, you'll want to use the express-csurf library, which you can find on
Github here: https://github.com/expressjs/csurf


Version 0.1.8 -> Version 0.1.9
------------------------------

**No changes needed!**


Version 0.1.7 -> Version 0.1.8
------------------------------

**No changes needed!**


Version 0.1.6 -> Version 0.1.7
------------------------------

**No changes needed!**


Version 0.1.5 -> Version 0.1.6
------------------------------

**No changes needed!**


Version 0.1.4 -> Version 0.1.5
------------------------------

**No changes needed!**


Version 0.1.3 -> Version 0.1.4
------------------------------

**No changes needed!**


Version 0.1.2 -> Version 0.1.3
------------------------------

**No changes needed!**


Version 0.1.0 -> Version 0.1.2
------------------------------

**No changes needed!**


Version 0.0.0 -> Version 0.1.0
------------------------------

**No changes needed!**
