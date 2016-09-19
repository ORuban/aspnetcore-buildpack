# aspnetcore-buildpack
ASP.NET Core 1.0 Buildpack

Sample of using this buildpack is availabel [here][3].

---


### How to use this buildpack to deploy your code on Heroku
Sample is available here
1. Create new Heroku App
2. Configure NodeJS environment. This step is only for apps that requeries to node, npm to be isntalled.

   Add [NodeJS buildpack][2]. To do so open "*Settings*" tab in Heroku App dashboard, go to "*Buildpack*" section and add next buildpacks:
   - heroku/nodejs
   
   then, if needed, install npm packages using package.json
   Example of package.json file used to install bower:
   ~~~~
   {
      "scripts": {
         "preinstall": "npm list bower -g || npm install bower -g",
         "postinstall": "bower cache clean"
      },
      "dependencies": {
      }
   }
   ~~~~
 
   >Keep in mind, that **NodeJS** buildpack always requires the package.json file. Add it to root of your repository even if you do not need other then node, npm dependencies.
   
   
3. Add ASP.NET-Core buildpack. Open "Settings" tab in Heroku App dashboard, go to "Buildpack" section and add next buildpacks: 
   - https://github.com/ORuban/aspnetcore-buildpack.git 

4. Add Heroku Config Variables
  
   Buildpack uses the following Config Variables
    * **LAUNCH_ASSEMBLY_NAME** - the assembly name, that will be used to run app via Kestrel
    * **PUBLISH_APP_DIR** - the relative path to main published project (relative path to folder that contains main project.json)

  > Config Variables should be added on "Settings" tab in Heroku app dashboard. Read more about Convig Variables in [Heroku documentation][1]

</b>
---

Troubleshooting: 
* IF you face with errors like "*bower command not found*", add calling "which bower" command before. Lets say your prepublish script looks like
~~~~
"prepublish": [ "bower install" ],
~~~~

Modify it to
~~~~
"prepublish": [ "which bower && bower install" ],
~~~~
[1]: https://devcenter.heroku.com/articles/config-vars
[2]: https://devcenter.heroku.com/articles/nodejs-support
[3]: https://github.com/ORuban/asp.net-core_deploy_on_heroku
