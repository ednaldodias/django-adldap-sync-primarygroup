# django-adldap-sync-primarygroup

Overhaul of django-adldap-sync in https://github.com/marchete/django-adldap-sync.
django-adldap-sync-primarygroup provides a Django management command that synchronizes LDAP users, groups and memberships from an Active Directory server. Including the primary group   of users.

** Replace the file https://github.com/marchete/django-adldap-sync/blob/master/adldap_sync/management/commands/syncldap.py


**It is necessary to add these parameters in the following configuration in settings.py

LDAP_SYNC_USER_EXTRA_ATTRIBUTES = ['primaryGroupID', 'objectSid']
