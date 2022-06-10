# sqldb_python_connection
Connect SQL isolated container of docker from python and perform some KPI 
- For docker isolated container you required to install docker on your system.In my case i am using docker desktop for windows.
- Next you need further configurate for SQL Server, such as
  - Open command prompt and execute this command,it download latest build from docker hub
    ```
    docker pull mcr.microsoft.com/mssql/server:2022-latest
    ```
  - Create docker container and run it by using this command
    ```
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=PASSWORD" -p 1401:1433 --name sql2 --hostname sql2 -d mcr.microsoft.com/mssql/server:2022-latest
    ```
  - Now open command promt in bash mode using this command
    ```
    docker exec -it sql2 "bash"
    ```
  - Open `sqlcmd` for further database changes
    ```
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "PASSWORD"
    ```
  - To create new database and create tables. use these commands
    ```
    CREATE DATABASE database_name
    GO
    SELECT name FROM sys.databases;
    GO
    USE database_name
    CREATE TABLE Table_name
    GO
    
    ```
 - Copy tsv file in docker container mount memory
   ```
   docker cp File_PATH\File_NAME.tsv sql2:/mnt/
   ```
 - Execute bulk query to import data fromTSV file to database tabels
   ```
   BULK INSERT Products FROM '/mnt/File_NAME.tsv' WITH (DATAFILETYPE = 'char', FIELDTERMINATOR = '\t', KEEPNULLS);
   ```
 Now sql db is ready to use in docker container. You can connect to databse by using `sql_db_connection.ipynb` from jupyter notebook.   
