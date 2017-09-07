# ejabberd Community Server 16.04 on Ubuntu 16.04 LTS

## Configuration for File Transfer capability - Step by step

After having single-user & Multi-user chat room capabilities configured, you might want ejabberd to be able to transfer files bewteen users.

You'll have to edit some snippet part of the configuration file **_ejabberd.yml_** to define this capability.

Open a command line and type the following:

  **1.** Open the /opt/ejabberd-16.04/conf/ejabberd.yml file with gedit or other text editor of your choice
  
  ```
  sudo gedit /opt/ejabberd-16.04/conf/ejabberd.yml
  ```

  **2.** Search in the modules section the following configuration:
    
  * You'll see this snippet part:
    ```
      modules:
        ...
          ## mod_http_fileserver:
  	      ##   docroot: "/var/www"
  	      ##   accesslog: "/opt/ejabberd-16.04/logs/access.log"
        ...
    ```
        
  * Then, you have uncomment it and add some configuration rules but at least add the following line, you'll have then this snippet configuration:
        
    ```
      modules:
        ...
          mod_http_fileserver:
              docroot: "/var/www"
  	          accesslog: "/opt/ejabberd-16.04/logs/access.log"
              default_content_type: "text/html"
        ...
    ```

   * Then, you have to search the following 173 line and define it as a handler in the HTTP service. 
    You'll see this snippet part enable:
     ```   
          -
  	        port: 5280
              module: ejabberd_http
              request_handlers:
                "/websocket": ejabberd_http_ws
              ##  "/pub/archive": mod_http_fileserver
              web_admin: true
              http_bind: true
              ## register: true
              captcha: false
        ```
     
        
    * Then, you need to uncomment the __##  "/pub/archive": mod_http_fileserver__ line
      ```   
          -
  	        port: 5280
              module: ejabberd_http
              request_handlers:
                "/websocket": ejabberd_http_ws
                "/pub/archive": mod_http_fileserver
              web_admin: true
              http_bind: true
              ## register: true
              captcha: false
        ```
        
**_So, you've done it. You could restart your server to have this configuration ready to use between clients._**
  
