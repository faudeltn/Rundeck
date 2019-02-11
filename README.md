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

- Modify the <code>/etc/rundeck/profile</code> as below:

Before
```

RDECK_JVM="-Djava.security.auth.login.config=$JAAS_CONF \
           -Dloginmodule.name=$LOGIN_MODULE \

```
After
```

RDECK_JVM="-Djava.security.auth.login.config=/etc/rundeck/jaas-activedirectory.conf \
           -Dloginmodule.name=activedirectory \

```

- Create the [rundeck_administrators.aclpolicy](https://github.com/faudeltn/Rundeck/blob/master/rundeck_administrators.aclpolicy/) and change the ownership to the rundeck user as below
```
# vi /etc/rundeck/rundeck_administrators.aclpolicy
chown rundeck:rundeck /etc/rundeck/rundeck_administrators.aclpolicy
chmod 640 /etc/rundeck/rundeck_administrators.aclpolicy
```

- Create the [rundeck_users.aclpolicy](https://github.com/faudeltn/Rundeck/blob/master/rundeck_users.aclpolicy/) and change the ownership to the rundeck user as below
```
# vi /etc/rundeck/rundeck_users.aclpolicy
chown rundeck:rundeck /etc/rundeck/rundeck_users.aclpolicy
chmod 640 /etc/rundeck/rundeck_users.aclpolicy
```

- Create the new roles by editing the file <code>/var/lib/rundeck/exp/webapp/WEB-INF/web.xml</code>
```
<security-role>
                <role-name>rundeck_administrators</role-name>
        </security-role>
        <security-role>
                <role-name>rundeck_users</role-name>
        </security-role>
```

- Finally restart the Rundeck daemon:
```
systemctl restart rundeckd
```
- To check the log file of rundeck use the below command:
```
tail -f /var/log/rundeck/service.log
```

Visit our website[YallaLabs](http://yallalabs.com/)

