# Rundeck Active Directory Integration



- Create a <code>jaas-activedirectory.conf</code> file as below:
```
# vi /etc/rundeck/jaas-activedirectory.conf
  activedirectory {
    com.dtolabs.rundeck.jetty.jaas.JettyCachingLdapLoginModule required
    debug="true"
    contextFactory="com.sun.jndi.ldap.LdapCtxFactory"
    providerUrl="ldap://IP_DOMAIN_CONTROLER:389"
    bindDn="CN=YOUR_BIND_USER,OU=Rundeck,OU=Application,DC=YALLALABS,DC=LOCAL"
    bindPassword="XXXXXXXXXXXXXXX"
    authenticationMethod="simple"
    forceBindingLogin="true"
    userBaseDn="DC=YALLALABS,DC=LOCAL"
    userRdnAttribute="sAMAccountName"
    userIdAttribute="sAMAccountName"
    userPasswordAttribute="unicodePwd"
    userObjectClass="user"
    roleBaseDn="OU=Rundeck,OU=Application,DC=YALLALABS,DC=LOCAL"
    roleNameAttribute="cn"
    roleMemberAttribute="member"
    roleObjectClass="group"
    cacheDurationMillis="300000"
    reportStatistics="true";
};
```

- Change the ownership of the file and set up the correct permission:
```
# chown rundeck:rundeck /etc/rundeck/jaas-activedirectory.conf
# chmod 640 /etc/rundeck/jaas-activedirectory.conf
```


