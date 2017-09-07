# ejabberd Community Server 16.04 on Ubuntu 16.04 LTS

## Installation - Step by step

**1.** Download the ejabberd Community Server 16.04 DEB package:

    wget https://www.process-one.net/downloads/ejabberd/16.04/ejabberd_16.04-0_amd64.deb
  
**2.** Install ejabberd Community Server DEB package:
    
    sudo dpkg -i ejabberd_16.04-0_amd64.deb
 
After the installation is finished, we need to configure an account to have access to the ejabberd Community Server.
This takes place by configuring the file ejabberd.yml, this file is at **/opt/ejabberd-16.04/conf/ejabberd.yml**

We'll edit the file using gedit, you could use any text editor of your choice...

**3.** Configure the ejabberd.yml file:
    
    sudo gedit /opt/ejabberd-16.04/conf/ejabberd.yml
    
  * Search the line 405 (this line defines the Access Control Lists, if you use gedit just press Ctrl+f and type acl, then press enter) and add the following:*
    * You'll see this snippet
      ```
      admin:
        user:
          - "admin": "ubuntu"
      ```
    * Change to this:
      ```
      admin:
        user:
          - "admin": "ubuntu"
		  - "username": "yourip"
      ```
    _In you "username" field type the name you want to use and in "yourip" type your computer ip address_
   * Then, search the line 89 (this line defines the SERVED HOSTNAMES, if you use gedit just press Ctrl+f and type hosts, then press enter till reach the line) and add the following:
	 	* You'll see this snippet
      ```
			hosts:
  			- "ubuntu"
      ```
		* Change to this:
      ```
			hosts:
    		- "ubuntu"
  			- "yourip"
      ```
		* _In "yourip" field type your computer ip address_

After doing that, you need to start the server and register the account

**4.** Start the server

    sudo /opt/ejabberd-16.04/bin/start

**5.** Finally, register the account to have acces to the server
    
    sudo /opt/ejabberd-16.04/bin/ejabberdctl register username yourip password
    
  *_Register the account with the username and yourip that you configured in the 3rd step_
  
  *_The 4th step will open your browser and you be able to access to the welcome main view to the server, you'll need to enter the credentials you registered_
