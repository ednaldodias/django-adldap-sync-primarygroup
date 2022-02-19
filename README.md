# django-adldap-sync-primarygroup

Overhaul of django-adldap-sync in https://github.com/marchete/django-adldap-sync.

django-adldap-sync-primarygroup provides a Django management command that synchronizes LDAP users, groups and memberships from an Active Directory server. Including the primary group   of users.

** Replace the file https://github.com/marchete/django-adldap-sync/blob/master/adldap_sync/management/commands/syncldap.py

** Modifications flagged with "####!!!!"


**It is necessary to add these parameters in the following configuration in settings.py

LDAP_SYNC_USER_EXTRA_ATTRIBUTES = ['primaryGroupID', 'objectSid']

# Error found:

---> comment the lines 672 to 676

```
    def get_ldap_user_membership(self, user_dn, primaryGroupSid):                               ####!!!!!
        """Retrieve user membership from LDAP server."""
        #Escape parenthesis in DN
        membership_filter = self.conf_LDAP_SYNC_GROUP_MEMBERSHIP_FILTER.replace('{distinguishedName}', user_dn.replace('\,', ',').replace('(', "\(").replace(')', "\)"))
        try:
            uri, groups = self.ldap_search(membership_filter, self.conf_LDAP_SYNC_GROUP_ATTRIBUTES.keys(), False, membership_filter)
        except Exception as e:
            logger.error("Error reading membership: Filter %s, Keys %s" % (membership_filter, str(self.conf_LDAP_SYNC_GROUP_ATTRIBUTES.keys())))
            return (None, None)
        #logger.debug("AD Membership: Retrieved %d groups for user '%s'" % (len(groups), user_dn))
        
        objectsid_filter =                                                              \
            '(objectSid=' + primaryGroupSid + ')'                                                    ####!!!
        uri1, groups1 = self.ldap_search(objectsid_filter,                              \
                                self.conf_LDAP_SYNC_GROUP_ATTRIBUTES.keys(),            \
                                   False, objectsid_filter)                                         ####!!!
        #objectsid_filter =                                                                 \        ####!!! correction
        #        '(&(objectClass=group)(member:1.2.840.113556.1.4.1941:=' + groups1[0][0] + '))'     ####!!! correction
        #uri1, groups1 =                                                                     \       ####!!! correction
        #        self.ldap_search(objectsid_filter, self.conf_LDAP_SYNC_GROUP_ATTRIBUTES.keys(), \   ####!!! correction
        #        False, objectsid_filter)                                                            ####!!! correction

        groups.extend(groups1)                                                                      ####!!!!

        return (uri, groups)

```
