# HUE and Active directory integration
#### hue.ini sample configuration
```sh
[desktop]
    [[auth]]
        backend=desktop.auth.backend.LdapBackend
    [[ldap]]
        base_dn="DC=mycompany,DC=com"
        ldap_url=ldap://mycompany.com
        nt_domain=mycompany.com
        bind_dn=cdw2authuser
        bind_password=password
        create_users_on_login = true
        search_bind_authentication=false
        [[[users]]]
		    user_filter="objectclass=*"
		    user_name_attr=sAMAccountName
```

#### Brief about active directory
In active directory, by default anonymous searches are not allowed. Hence the username and password provided by you during login is used for binding to the Active Directory server. If the username/password is wrong, the authentication fails and you can't login to the application. Let's see how I come up with the above configuration for HUE to connect to Active Directory server.

#### Hue.ini config
```Base DN``` : The base domain under which all the users under this domain are allowed to login.

```Ldap url``` : The URL of LDAP server. The default port 389 is used if not specified.

```NT Domain``` : For active directory integration, the config nt_domain doesn't exist in hue.ini file. It has to be added to hue.ini. If your email identity is xyz@mycompany.com, then nt_domain gets the value "mycompany.com".
> While authenticationthenticating the username provided ('xyz') will be framed as <username>@<nt_domain> (xyz@mycompany.com) and will be used as userPrincipalName (UPN). The password provided during login is used for binding.
Note : nt_domain is used only for authenticating active directory server

```create_users_on_login``` : Setting true will create the user in Hue once the LDAP authentication succeeds. (ie) Set it to true if you want to allow access to all the users under Base DN. Setting it as false will restrict the HUE access to users who were imported by the Hue super user.

```search_bind_authentication``` :  There are two ways to bind to a LDAP server.
- Search bind
- Direct bind
 
In our case we are going to bind to the Active directory server using the username and password provided by the user, which comes under direct bind. 

```user_filter``` : attribute used to filter the search we do. "objectclass=*" value searches all the elements under the baseDN. If we specify something like "objectclass=person", only elements with type person is searched.

```user_name_attr``` : The attribute in LDAP that contains the username.

```bind_dn``` and ```bind_password``` : Needed for importing users in a group. Used only when anonymous searches are not allowed in active directory.

#### Sources
- https://groups.google.com/a/cloudera.org/d/topic/hue-user/D2ttiFnHkJo/discussion
- http://gethue.com/making-hadoop-accessible-to-your-employees-with-ldap/
