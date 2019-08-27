### Configuration and installation of PostgreSQL and PostGIS.

### Introduction.
  
#### PostgreSQL 

 It is a free and open source database. PostgreSQL runs on all major operating systems.


#### PostGIS

PostGIS is an open source,freely available,and fairly OGC compliant spatial database extender for the PostgreSQL Database Management System.In a nutshell it adds spatial functions such as distance, area, union, intersection, and specialty geometry data types to the database.

 For our project we need to easily access,manage and update geospatial information on our database (PostgreSQL) thus we need the extension PostGIS as we deal with geospatial data.

### Installation Procedures.

1. PostgreSQL AND PostGIS installation.

The installation of Postgres and Postgis involves the use of the Linux system commands and as from the basis the following should be known:

- cd: used for opening file directories.

- ls: used for listing components present in a File directory.

- sudo: used instead of cd to perform various function such as installation, opening files within a file directories.

To install PostgreSQL and PostGIS, run a system update and upgrade all necessary files using the following command:

       `$sudo apt-get update`

       `$sudo apt-get upgrade`

- The above command are there to perform an update and upgrade the existing files in the system for the incomming installation.

- Upon this the main errors that can be encountered are:

        1. Writing Errors.

        2. Spacing Errors.

After performing the above commands assuming no error was encounterd proceed as follows:

- ***Installing Postgress and Postgis***

On installing after upgrading and updating the system run the following commands:
   
     `$sudo apt-get install postgresql postgresql-contrib postgis`

This will install PostgreSQL and PostGIS.

2. Creation of a a new Database.

- Modify the default user on the database server using psql.
	
      `sudo -u postges psql`
 
  Then change the password for the user postgres by typing:
	\password postgres      Required to enter the password twice, then you can create a new user as:

	# createuser --superuser $USERNAME OR
	
        # createuser --interactive $USERNAME.

NOTE:It does not mean that username must be in capital letter.

Quit psql with the command:

	\q

Can not be achieved if not yet install postgres.


- Create a new database,there are two steps here and what required is to be in postgres directory;
	
        # CREATE DATABASE  $name of the database OR
	
        # createdb $name of the database
    
Run the command \l to ensure that database is created successful then you will see the database is owned by postgres.

- By default database name is in lower case so whenever write in either upper case or lower case is saved to be lower case.

- When creating a database make sure that you are in postgres directory otherwise it will fail to create a database.


3. How to create Extension. 

- Enter psql as user postgres and connect to the newly created database.

     `sudo -u postgres psql database_name`

- Run the following commands to enable PostGIS on the gis database:

     CREATE EXTENSION postgis; Enables PostGIS with raster support

     CREATE EXTENSION postgis_topology; Enables topology

     CREATE EXTENSION postgis_sfcgal; Enables PostGIS Advanced 3D and other geoprocessing algorithms

Note that you will need to enable the PostGIS extensions on each new database
. 
Hence, if you create a new database for a project, you will need to enable the extensions after creating the database.

4. Edit the postgresql.conf and pg_hba.conf file.

- First you have to change directory
    
    `cd /etc/postgresql/10/main/`

 changing directory is essential otherwise you won't be able to edit the files.   

- if you write the command `ls` you will see the postgresql.conf and pg_hba.conf files

 Note: By default, PostgreSQL only enables connections from the localhost. To enable remote connections (e.g. from your host machine or another machine on your network), two config files need to be modified, namely ***postgresql.conf*** and ***pg_hba.conf***.

To enable remote access to the database server, you must first add a listening address. To listen to any address (enter star), edit the postgresql.conf file with the following command:

 `sudo nano /etc/postgresql/10/main/postgresql.conf`

- if you run the previous command and the file appears to be empty, you did something wrong. Ensure that your PostgreSQL version is the one written there.on our case our verion was 10 or you simply mistyped the command. Exit nano with ctrl-x, do not save the changes, and try again.

Once in the ***postgresql.conf file***, scroll down to the Connection Settings section and change the line that reads:

#listen_addresses = ‘localhost’

To…

listen_addresses = '*'

Make sure you remove the # at the start of the line. Press ctrl-x to exit. Save the changes when prompted.

To grant access to the database server for all users, edit the pg_hba.conf file and add the line ‘host all all 0.0.0.0/0 md5’.

Edit the ***pg_hba.conf file***

  `sudo nano /etc/postgresql/9.5/main/pg_hba.conf`

Add the following line at the end of the document:

***host all all 0.0.0.0/0 md5***


Exit nano with ctrl-x. Save changes when prompted. Note that this will allow any named user to access the database from any IP address. 

In order for these changes to take effect, PostgreSQL needs to be restarted. Do so with the following command:

  `sudo service postgresql restart`. This command is essential otherwise the test configuration will not work.

5. Test configuration in QGIS

In this optional step, you can test the connection from the host machine if you have QGIS installed. First, you need to determine the local IP address of the GIS test server. In the server’s terminal, get the current IPV4  address with the following command:

- Write ifconfg command

The IP needed is after ‘inet addr’.

#### In QGIS:

- locate the Browser Panel and right-click on ***PostGIS.***

   Click “***new connection***”

- Enter a name for the connection (e.g. GIS test server), the IP address for the Host (inet addr determined in previous step), do not change the port from 5432, and use gis as the Database.

- Enter ***postgres*** as the username and its password — do not mistakenly use the server password here as you need to use the one associated with the ***postgres*** user. Check both to save the credentials (again, not secure, but fine for a test server).

If all goes well, when you click on the Test Connection button, you should see “Connection to gis was successful”.

Things to note: database names, usernames, and passwords are all case sensitive with PostgreSQL. I try to use lowercase words consistently throughout the database for every database, table, schema, group, and user. If for some reason you cannot connect to the database, go back to the beginning of this tutorial and make sure you followed each step.

At this point, the geodatabase should be up and running.

### Links

- [installation_guide] (http://cliffpatterson.ca/blog/2018/07/06/creating-an-open-source-gis-test-server-part-2-installing-and-configuring-postgresql-postgis/)

- [Postgresql] (https://en.wikipedia.org/wiki/PostgreSQL)

- [PostGIS] (https://medium.com/@tjukanov/why-should-you-care-about-postgis-a-gentle-introduction-to-spatial-databases-9eccd26bc42b)