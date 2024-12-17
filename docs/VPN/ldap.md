# Ajout d'authentification ldap sur openvpn


dans ***/etc/openvpn/auth-ldap.conf***

```conf
<LDAP>
        URL             ldap://172.28.127.10

        BindDN          "CN=svcovpn,CN=Users,DC=lan,DC=chartres,DC=sportludique,DC=fr"

        Password        "password"

        Timeout         15
        TLSEnable       no
        FollowReferrals no
</LDAP>

<Authorization>
    BaseDN          "OU=services,DC=lan,DC=chartres,DC=sportludique,DC=fr"
    SearchFilter    "(sAMAccountName=%u)"
    RequireGroup    false

#    <Group>
#        BaseDN      "CN=Users,DC=lan,DC=chartres,DC=sportludique,DC=fr"
#        SearchFilter    "(objectClass=*)"
#        MemberAttribute "member"
#    </Group>

</Authorization>
```