==============
django_geoaxis
==============

A Django authentication backend for `GeoAxIS <https://gxisaccess.nga.mil>`_ OAuth.


requirements
^^^^^^^^^^^^

.. code-block::

   pip install social-auth-app-django


local_settings.py (example)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    import os
    from django.conf import settings

    def isValid(v):
        if v and len(v) > 0:
            return True
        else:
            return False

    SOCIAL_AUTH_NEW_USER_REDIRECT_URL = '/'
    DEFAULT_AUTH_PIPELINE = (
    'social_core.pipeline.social_auth.social_details',
    'social_core.pipeline.social_auth.social_uid',
    'social_core.pipeline.social_auth.auth_allowed',
    'social_core.pipeline.social_auth.social_user',
    'social_core.pipeline.user.get_username',
    'social_core.pipeline.mail.mail_validation',
    'social_core.pipeline.social_auth.associate_by_email',
    'social_core.pipeline.user.create_user',
    'social_core.pipeline.social_auth.associate_user',
    'social_core.pipeline.social_auth.load_extra_data',
    'social_core.pipeline.user.user_details'
    )
    SOCIAL_AUTH_GEOAXIS_KEY = os.getenv('OAUTH_GEOAXIS_KEY', None)
    SOCIAL_AUTH_GEOAXIS_SECRET = os.getenv('OAUTH_GEOAXIS_SECRET', None)
    SOCIAL_AUTH_GEOAXIS_HOST = os.getenv('OAUTH_GEOAXIS_HOST', None)
    OAUTH_GEOAXIS_USER_FIELDS = os.getenv(
        'OAUTH_GEOAXIS_USER_FIELDS', 'username, email, last_name, first_name')
    SOCIAL_AUTH_GEOAXIS_USER_FIELDS = map(
        str.strip, OAUTH_GEOAXIS_USER_FIELDS.split(','))
    OAUTH_GEOAXIS_SCOPES = os.getenv('OAUTH_GEOAXIS_SCOPES', 'UserProfile.me')
    SOCIAL_AUTH_GEOAXIS_SCOPE = map(str.strip, OAUTH_GEOAXIS_SCOPES.split(','))
    ENABLE_GEOAXIS_LOGIN = isValid(SOCIAL_AUTH_GEOAXIS_KEY)
    if settings.SITEURL.startswith('https'):
        SOCIAL_AUTH_REDIRECT_IS_HTTPS = True
    # GeoAxisOAuth2 will cause all login attempt to fail if
    # SOCIAL_AUTH_GEOAXIS_HOST is None
    if ENABLE_GEOAXIS_LOGIN:
        settings.AUTHENTICATION_BACKENDS += (
            'django_geoaxis.backends.geoaxis.GeoAxisOAuth2',
        )

