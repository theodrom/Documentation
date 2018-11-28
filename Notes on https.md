# Notes on make a site run on https:

On an episerver project, to be able to log in, you have to:

- Before following the steps below, go to Epi-project, admin/config - manage webpages and add the localhost:(https port) or the domain name with https protocol. Optional to make it primary. **P**

- Install URLRewrite on IIS (for version 2.0) from here <https://www.iis.net/downloads/microsoft/url-rewrite>. (Works With: IIS 7, IIS 7.5, IIS 8, IIS 8.5, IIS 10). **P**

- Add on project's web.config file this snippet (inside the system.webServer): **P**
  ````
      <rewrite>
        <rules>
          <clear />
          <rule name="Redirect to https" stopProcessing="true">
            <match url="(.*)" />
            <conditions>
              <add input="{HTTPS}" pattern="off" ignoreCase="true" />
            </conditions>
            <action type="Redirect" url="https://{HTTP_HOST}{REQUEST_URI}" redirectType="Permanent" appendQueryString="false" />
          </rule>
        </rules>
      </rewrite>
  ````

  See here:  
  <https://world.episerver.com/forum/developer-forum/-Episerver-75-CMS/Thread-Container/2016/3/how-to-serve-episerver-cms-over-https/>  
  and here:  
  <http://jondjones.com/learn-episerver-cms/episerver-developers-guide/episerver-security/how-to-make-your-episerver-website-run-via-https>

- Create a Self Signed Certificates In IIS, see here:  
<https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html>  

- Change all the external referencies to js, css, etc. to https. **P**

- Change the indexing service url on the web.config file. **P**

- On production- if it doesn't exist - create a bindind of the page, that points to a http protocol. **P** 

## To enable https on Visual Studio

- Set SSL to true on project's properties. 
- Set the project URL to https://(domain name + portNumber) to run through LocalIIS.


Use these steps **(P)** to enable https on production.