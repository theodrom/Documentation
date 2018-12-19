## Installation in production envieronment of Epi and Imagvault, after the upgrading of both projects.  

### **1) Steps before moving the upgraded material to remote server**  


* Download the production databases, both for Epi and IV. The databases have to **get upgraded** to the respective versions of the local projects.

    **OBS: It is possible to upgrade the Epi-database on the production environment. See on 'working on remote server' below**

* Be aware of the **production-Server version**. The databases have to be restored and re-produced at the same SQL-Server version instance, otherwise thay wont't be restored to the production-Server.  

* Create new databases with differnt names than the _old databases_ from the server. These names will follow the databases, when you 'll install them later, in the production-Server and for safety reasons it's good to be different. **Restore and overwrite** the _new databases_ with the _old databases_.    
    **OBS**: Create a new user for the databases to log in with it.   



#### Imagevault

* On the existing IV web instance on IIS _(if there is no instance, create one)_, point the new database and run the application. Normally, the IV installation process begins, if not, add this url extension **/setup/?configure=1** after the main url. You need to be a user with the Administrator GlobalRole to be able to access the setup page.  
 

* Follow the steps for the set up, more info here: <https://www.imagevault.se/en/documentation/?v=v5.0&page=installation/imagevault5setup.html>

#### EPiServer project

* Change the connection string to work against the  _old database_.  

* When you run the page, normally, an error of the database's version will come up. On VS Studio => package manager console, run this command: 
    ```
    update-epidatabase
    ```

* Refresh the page and, normally again, a second error will appear about the **Utc**. Run this command:
    ```
    Convert-EPiDatabaseToUtc
    ```

* Update the IV packages, if necessary, to match the version of the IV instance.  

* Open the project in the browser and test if everything works fine. Possible errors:

    - The search function doesn't work inside the project. Check this page: <https://world.episerver.com/blogs/Eric-Pettersson/Dates/2014/5/Are-you-also-having-trouble-with-EPiServer-Search-IndexingServicesvc-and-SSL/>

    - No Imagevault pictures or access. The IV ````<sdkIdentity key="" secret="">```` must match with the production's database respective file.  

* Publish the project in release mode but first, make sure that all _.config_ files are included in the project. This will gather all the necessary files that are used by the app.  
    - Files required: **Web.config, episerver.config, episerverFramework.config, episerverLog.config, imagevault.client.config**.
      
    **OBS**: If a file is not included in the release folder, select the file, then in the Properties pane, set the Build Action to Content by choosing Content form the dropdown next to the property label. You can copy the missing files to the _release folder_ or publish the project again. 

----------

### **2) Steps on working with remote server**

* Place the upgraded project-folders somewhere beside the old ones.  

* Copy the _license_ files, both for Epi and IV, from the old project and core to the new installation folders.

* For https sites, check if the URLrewrite-extension is installed on IIS. If not, get it from here: <https://www.iis.net/downloads/microsoft/url-rewrite>  

If you have upgraded the databases localy:  

* Get backups of the remote(production) databases. 

* Restore the new upgraded databases with the new names. It is possible to use the old logins here.

* Correct the connection strings for Epi and IV.

#### Upgrading the Epi-database on the production environment  

* See here for more details:  
<https://world.episerver.com/documentation/Items/Installation-Instructions/Installing-Episerver-updates/updating-configuration-and-database-schemas/>  

* To get the above thing to work, you have to cd (point the folder) where the .bat file lives.

* In case you want to convert the database to UTC, follow these instructions:  
 <https://tedgustaf.com/blog/2016/upgrade-to-episerver-10/>

The following steps apply to both test-sites and/or production-sites.


#### EPiServer project

* Go to the Epi-release folder and modify it's _.config_ files to match the ones from the server. Possible configurations:
    - EpiserverFramework.config  
        - Replace all the **local paths** with the paths from the remote server.  
        - Check if the **paths to ui**, match. Better keep the ones from the server.  
            
    - Episerver.config  
        - Check if the **Url** to ui, match. Check also, for other possible paths that should be changed. 
        
    - Web.config  
        - Check and compare the **authorization method** between the Epi-project and the IIS.
        - Check the **cookie name**, if it's the same with the old one.  
        - Change the **machineKey** to match the server's machine key.  
        - In case of https, activate the **rewrite/rule**.  
        - Check again the **path to UI** to match with the server's.  
        - Check all the **authorization** and **role-names** to match with the server's. 
        - Check the **path in episerver.search** to match. 
        - Check the **path in episerver.search.indexingservice** to match.  
        - Check the **smtp** adress. Change to the client's. 
        
     
#### Imagevault

* Possible configurations on ImageVault's project.  
    - Change the **sdkIdentity key="" secret=""**. Take the ones that corresponds to the production database.
    - Change the **machineKey** to match the one in web.config.  
    - Change the **connectionString** to the new database.  
    - Change the paths to the **App_Data** folders. In case there are no App_Data folders, create them.  
    - Check the **cookie name** to match with the one in the project's web.config.  
    - Change the domain name in **membership** and **roleManager** tags.  

**OBS: Changes in the web.config files require to restart the app-pools on the IIS.**  

----------

### **3) Redirecting to the new installation folder**

* Go to IIS, on the page-application, change the Imagevault's **physical-path** to point the new installation. You may stop the respective service and, if all goes fine, inactivate it. Change the Epi's project **physical-path** to point the new one.
    

 
