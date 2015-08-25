Set up HUE
==========

1) Ensure that hue.ini is configured with the correct values. Sample config file :

	[desktop]
    	[[auth]]
        	backend=desktop.auth.backend.AllowFirstUserDjangoBackend
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

   Start Hue and login with the admin/admin credentials. It creates the superuser.

2) Now click on "Manage Users" link and the  select the "Groups" link on the top of the page. Then click on the "Add/Sync LDAP group". Type in the group that is supposed to access Hue. Select "Import new members" option and click on "Add/Sync group" button. It will create the group in Hue and add the users in the group to Hue. 

3) Revoke all permissions from the default group in Hue. Assign the necessary permissions for the imported group from LDAP.

4) Select the user who should be given the superuser access. It takes you to "Edit User" page. Assign a dummy password and select the "Superuser status" option in step 3. Then update the user.

5) Now change the hue.ini file as follows:

	[desktop]
		[[auth]]
			backend=desktop.auth.backend.LdapBackend
		[[ldap]]
			create_users_on_login = false

   Restart Hue.

5) Now login with the ldap username(user assigned superuser status from Step 3) and go to "Manage Users" page. There delete the user admin.

6) That's it. Now any user imported in Step 3 can login to Hue with his/her LDAP credentials.

7) In case a new user/group needs to be added, login as the hue superuser(user in Step 3), and then import the user/group.