Security in Embedded Cache
========================

This web application demonstrates how security works in upcoming Infinispan version, in embedded mode. Please give the following links a good reading to understand further how security in Infinispan works:

* https://github.com/infinispan/infinispan/wiki/Security
* http://blog.infinispan.org/2014/04/infinispan-security-1-authorization.html
* [Official Documentation](https://access.redhat.com/documentation/en-us/red_hat_jboss_data_grid/7.1/html-single/administration_and_configuration_guide/#securing_data_in_red_hat_jboss_data_grid)

Prerequisites
-------------
1. JDK 1.8+
2. Maven 3.5
3. JBoss EAP 7.x

Setup
-----
1. Ensure that the local JBoss server is running in Standalone mode
2. To build and package the webapp, run the command `mvn clean package` at the command prompt in the root directory of the project
3. To deploy the __security-domain__ that will be used for Authentication, run the command `mvn jboss-as:execute-commands`
4. To deploy the packaged webapp, run the command `mvn jboss-as:deploy`
5. Since we will be using __application-user.properties__ and __application-roles.properties__ files that come with a standard JBoss server installation at path: __$JBOSS_HOME/standalone/configuration__, run the following commands from the `bin` folder of the server installation
        
   ```sh       
   # Add a user who will be a reader. A reader can only read from the cache and cannot perform cache modifications
   ./add-user.sh -a -u readerUser -p readerUserPass9! -r ApplicationRealm -g reader
   
   # Add a user who will be an admin. An admin can perform ALL possible operations on the cache
   ./add-user.sh -a -u adminUser -p adminUserPass9! -r ApplicationRealm -g admin
   ```		
6. Restart the application server to ensure that additions to the files containing the users/roles will be picked up
7. Considering a very basic setup of the server, the application should now be accessible at the URL: http://127.0.0.1:8080/secure-embedded-cache-quickstart/

Testing
-------
1. Log in as __readerUser__ when prompted for a login
2. Once successfully authenticated, using the form on the page presented to you, try adding a string Key/Value pair. Make a note of any messages displayed.
3. Now, log out <sup>1</sup> as __readerUser__ by using the link provided in the location of your browser OR by clicking on it: [http://logout@127.0.0.1:8080/secure-embedded-cache-quickstart/](http://logout@127.0.0.1:8080/secure-embedded-cache-quickstart/)
4. You will be prompted with a login again, at which point, click on __Cancel__ button
5. Now try logging in as __adminUser__ by clicking on the original URL (step 7 of Setup)
6. Repeat testing step #2. If you see the writes being permitted, go ahead and add 5 new entries and delete 2 of them as part of testing
7. Now logout as __adminUser__ using the URL provided in testing step #3 and in the same manner as described above
8. Log back in as __readerUser__ and verify that you see 3 entires in the cache. If Yes, the testing was a SUCCESS. While still logged in as __readerUser__, see if you could delete any entries from the cache and note any messages displayed

References
----------
<b><sup>1</sup></b> The above shown log out steps work well with Firefox. If it doesn't work well in your environment, you have three choices to log in as a different user:

1. Start a new browser session  (applies to IE 6+)
2. Use another browser 
3. Kill/Start (restart) the browser 
