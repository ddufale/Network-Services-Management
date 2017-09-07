# ejabberd Community Server 16.04 on Ubuntu 16.04 LTS

## Configuration for Multi-User Chat (chat room) capability - Step by step

When you install ejabberd Community Server you might want to have de Multi-User Chat capability available, it needs to have the module "mod_muc_admin: {}" defined.
By default, the installation brings this enabled. 

**But you'll have to edit some snippet part of the configuration file to define the service chat room name.**

Open a command line and type the following:

  1. Open the /opt/ejabberd-16.04/conf/ejabberd.yml file with gedit or other text editor of your choice
  ```
  sudo gedit /opt/ejabberd-16.04/conf/ejabberd.yml
  ```
  
  2. Search the line 594 or the line that says mod_muc, you'll see the following snippet:
  
  ```
  mod_muc: 
    ## host: "conference.@HOST@"
    access: muc
    access_create: muc_create
    access_persistent: muc_create
    access_admin: muc_admin
  mod_muc_admin: {}
  ```
    
  3. Change this snippet to:
  
  ```
  mod_muc: 
    ## host: "conference.@HOST@"
    host: "conference.yourip"
    access: muc
    access_create: muc_create
    access_persistent: muc_create
    access_admin: muc_admin
  mod_muc_admin: {}
  ```
  
  _Type your yourip address that you configured in the 3rd step second part in the [Installation & Setting up](https://github.com/ddufale/Network-Services-Management/blob/master/XMPP%20Server%20with%20ejabberd%20Community%20Server%2017.04%20on%20Ubuntu%2016.04%20LTS/Installation%20%26%20Setting%20up.md)_, _save and exit_.

4. Type the following lines on the terminal:
    * Create a room chat
      ```
        sudo /opt/ejabberd-16.04/bin/ejabberdctl create_room room_name muc_service xmpp_domain
          
        Example: sudo /opt/ejabberd-16.04/bin/ejabberdctl create_room adser conference.yourip yourip
      ```
    * Send a direct invitation to a specific user
      ```
        sudo /opt/ejabberd-16.04/bin/ejabberdctl send_direct_invitation adser conference.yourip none "Join the multi-chat" user@yourip
      ```
	
    * Set the affilation (level within the chat room) of the user you sent the invitation for the chat room you already created
      ```
        sudo /opt/ejabberd-16.04/bin/ejabberdctl set_room_affiliation adser conference.yourip user@yourip [owner, admin, member, outcast, none]
	    ```
    * Finally, you could retrieve the affilations to the chat room you created
      ```
        sudo /opt/ejabberd-16.04/bin/ejabberdctl get_room_affiliations adser conference.yourip
      ```
     
  
  
