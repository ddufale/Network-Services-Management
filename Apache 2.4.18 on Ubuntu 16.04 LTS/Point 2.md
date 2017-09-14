
## 2. Three different sites by different listen ports

Prerequisite. [Apache Server 2.4.18 Installation](https://github.com/ddufale/Network-Services-Management/blob/master/Apache%202.4.18%20on%20Ubuntu%2016.04%20LTS/Install%20Apache%202.4.8.md)
##

Our root document (the main directory on which Apache will search our content to show) will be configured with individual 
directories within the **/var/www** path. We'll create our directories for our three virtual containers on different ports 
we want to configure.

  **1.** Open a terminal and type the following lines to create our directories to be configured
  
      sudo mkdir -p /var/www/point_2/cerveza.com/public_html
      sudo mkdir -p /var/www/point_2/vodka.com/public_html
      sudo mkdir -p /var/www/point_2/whiskey.com/public_html

_"/point_2" refers to the second task to be achieved so that we will create a individual directory for this_

If you are using some different user from root, you need to modify the current user read permissions over our created directories 
to be able of deploying their content. If you are not, go to the third step.

  **2.** Type the following line on the terminal
      
      sudo chown -R $USER:$USER /var/www/point_2/cerveza.com/public_html
      
  _Do the same for the remaining directories_
  
  **3.** Use the following line to change the read permissions to our main directory

      sudo chmod -R 755 /var/www/point_2/
      
Now, our server has the necessary permissions to deploy his content and to be edited by the current root user or other.

  **4.** Create a test page for each virtual container
  
   * Type the follwoing lines to create and add some html content the test pages for each virtual container
            
            sudo nano /var/www/point_2/cerveza.com/public_html/index.html
            
   * HTML content to be added
            
            <html>
              <head>
                <title>Bienvenido a cerveza.com!</title>
              </head>
              <body>
                <h1>Éxito! El Virtual Host cerveza.com esta funcionando!</h1>
              </body>
            </html>
            
  __** Do the same for the remaining directory ("vodka.com" and "whiskey.com"), but not forget to change the content
  to show different pages.**__

The Virtual Host files contain specific information and configuration for each domain and aim to Apache how to respond
for specific requests to different domains adn ports.


  **5.** Create the configuration file for each virtual host corresponding to each domain name
  
   * Type the following line to create the file
        
          sudo nano /etc/apache2/sites-available/cerveza.com.conf 
   
   * Add the following content to the file created
          
          <VirtualHost *:81>
            ServerAdmin admin@cerveza.com
            ServerName cerveza.com
            ServerAlias www.cerveza.com
            DocumentRoot /var/www/point_2/cerveza.com/public_html
            ErrorLog ${APACHE_LOG_DIR}/point_2/cerveza.com/error.log
            CustomLog ${APACHE_LOG_DIR}/point_2/cerveza.com/access.log combined
          </VirtualHost>
    
    * Save, exit 
    * Last, create a indivual directory to place the error log and access log corresponding to each domain name 
    
            sudo mkdir -p /var/log/apache2/point_2/cerveza.com
            
    * _do the same for the remaining sites (vodka.com and whisley.com) and do not forget to change the port on every file_
    
  **6.** Then, you need to define the ports you want to listen corresponding to each domain name
  
   * Open the /etc/apache2/ports.conf file by typing the following line
   
          sudo nano /etc/apache2/ports.conf 
        
   * Add the following lines below the "Listen 80" line
        
            ...
              Listen 80
              Listen 81
              Listen 82
              Listen 83
            ...
            
   * Save and exit
  
  **7.** Then, you need to enable these sites by typing the follwing lines
      
      sudo a2ensite cerveza.com.conf 
      sudo a2ensite vodka.com.conf 
      sudo a2ensite whiskey.com.conf 
    
      
   You will need to reload Apache Server by typing the follwing line on terminal
        
        sudo service apache2 restart
  
  **8.** After that, you need to add some lines to your **/etc/hosts** file 
  
   * Open the /etc/hosts file by typing the following line
      
          sudo nano /etc/hosts
   
   * Then, add these lines to especify your IP address and the domain name to be mapped
   
            ...
            yourip    www.cerveza.com
            yourip    www.vodka.com
            yourip    www.whiskey.com
            ...
          
   Save and exit.
   
  **9.** Lastly, you only need to test with the browser entering each domain name and the corresponding port.
   
   
   
   
   
   