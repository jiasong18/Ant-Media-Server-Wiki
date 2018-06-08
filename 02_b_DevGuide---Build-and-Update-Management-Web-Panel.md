1. Go to the Management Web Panel Project Directory

    ```
    cd path/to/the/ManagementConsole_AngularApp/
    ```

2.  Build the Management Panel for Production
    ```
    ng build --prod
    ```
After this command finishes, there will be folder with name 'dist'. 
Now it is time to replace the content in Ant Media Server project

3. Go to the Ant-Media-Server project root folder and remove all files
    ```
    cd path/to/the/Ant-Media-Server/src/main/server/webapps/root
    rm -rf *
    ```

4. Copy the dist folder of Management Panel to the root folder of Ant-Media-Server
   ```
   cp -R path/to/the/ManagementConsole_AngularApp/dist/*  path/to/the/Ant-Media-Server/src/main/server/webapps/root
   ```

5. Repackage server and test the management panel and If everything is ok, commit and push the changes

