

# PostgreSQL
![postgresql_logo](https://miro.medium.com/max/800/1*PY24xlr4TpOkXW04HUoqrQ.jpeg)

## Table of Contents

 - [What Is Postgresql](https://github.com/mechano59/PostgreSQL/blob/master/README.md#what-is-postgresql)
 - [Prerequisite](https://github.com/mechano59/PostgreSQL/blob/master/README.md#prerequisite)
 - [Installing PostgreSQL](https://github.com/mechano59/PostgreSQL/blob/master/README.md#installing-postgresql)
 - [Knowing Things About PostgreSQL](https://github.com/mechano59/PostgreSQL/blob/master/README.md#knowing-things-about-postgresql)
 - [Helpful Commands](https://github.com/mechano59/PostgreSQL/blob/master/README.md#helpful-commands)
 - [Off You Go](https://github.com/mechano59/PostgreSQL/blob/master/README.md#off-you-go)

## What Is Postgresql
PostgreSQL is a powerful, open source object-relational database system that uses and extends the SQL language combined with many features that safely store and scale the most complicated data workloads. The origins of PostgreSQL date back to 1986 as part of the POSTGRES project at the University of California at Berkeley and has more than 30 years of active development on the core platform.
## Prerequisite
You need to have Administrator privileges in order to properly install this software.
And it's is always a good idea to update your machine before doing any installation to avoid any errors. For that , use :

    sudo apt update
    sudo apt upgrade

## Installing PostgreSQL
Now that that's out of the way, let's install it, shall we? 
It is available for many distributions, but I will only be showing how to install it on a linux (Ubuntu) machine. 

**Note that** : If you are using arm64 or i386 architectures you need to have Ubuntu 18.04 (Bionic Beaver) or older. Otherwise you can just install it on 20.04 (Focal Fossa) too. And its better to have a LTS version. 

Create the file repository configuration:

    sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
Import the repository signing key:

    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

![create_and_import_repo](https://github.com/mechano59/PostgreSQL/blob/master/images/create_and_import_repo.png)

Update the package lists:

    sudo apt-get update  
Install the latest version of PostgreSQL.

    sudo apt-get install postgresql postgresql-contrib
**Tip** : If you want to install a specific Version just write 

    sudo apt-get install postgresql-x postgresql-contrib-x
'x' being the particular version you want.

## What To Do After Installation
You can now access a Postgres prompt immediately by typing:

    sudo -i -u postgres psql
You can exit out of the PostgreSQL prompt by using :

postgres=# `\q`

But before quitting it's a good idea to set a password for the default postgres as there is no password set by default. For that use :

postgres=# `\password postgres`

Enter new password : `123456`

Enter it again : `123456`

  postgres=# `\q`
  

**Use a strong password please.  :3** 

![set_postgres_password](https://github.com/mechano59/PostgreSQL/blob/master/images/set_postgres_password_2.png)

Now it's not recommended practice to use the default postgres role for everything. 
So let's make a new role. For that use : 

    sudo -u postgres createuser --interactive --password
Enter name of role to add: `plaban`

Shall the new role be a superuser? (y/n) : `y`

Password: `123456`  
***please use something stronger.***

![create_user_postgres](https://github.com/mechano59/PostgreSQL/blob/master/images/create_user_postgres.png)

Now in order to access this role we need to have a database of the same name. So let's create that database :

    sudo -u postgres createdb plaban --owner plaban
   Now you might think that you can access it right now, well, not quite. Not yet but soon. 
You see, to log in with  `ident`  based authentication like in our case `plaban`, you’ll need a Linux user with the same name as your Postgres role and database. To make that  use :

    sudo adduser plaban
Adding user 'plaban' ...

Adding new group 'plaban' (1002) ...

Adding new user 'plaban' (1002) with group 'plaban' ...

Creating home directory '/home/plaban' ...

Copying files from '/etc/skel' ...

Enter new UNIX password: `123456`

Retype new UNIX password: `123456`

passwd: password updated successfully

Changing the user information for plaban

Enter the new value, or press ENTER for the default

	Full Name []: 
	
	Room Number []: 
	
	Work Phone []: 
	
	Home Phone []: 
	
	Other []: 
	
Is the information correct? [Y/n] `y`

![create_user_pc](https://github.com/mechano59/PostgreSQL/blob/master/images/create_user_pc.png)


And its done. you can now easily access using that role by typing :

    sudo -i -u plaban psql

![plaban_login](https://github.com/mechano59/PostgreSQL/blob/master/images/plaban_login.png)


## Knowing Things About PostgreSQL

### Creating a Database

To create a database named general, we will use :

    CREATE DATABASE general WITH OWNER plaban;

We can see our list of database by using : 
`\l`

![create_database_general](https://github.com/mechano59/PostgreSQL/blob/master/images/create_databaase_general.png)

We can use `\conninfo` to see that we are connected to database plaban as user plaban.  To change our current database and use the new database we will use :

    \c general 

![connect_to_general](https://github.com/mechano59/PostgreSQL/blob/master/images/connect_to_general.png)

### Creating a Table 

To create a table use :

    CREATE TABLE students (
    roll BIGSERIAL NOT NULL PRIMARY KEY, 
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    gender VARCHAR(8) NOT NULL, 
    parent_contact VARCHAR(50),
    department VARCHAR(50) NOT NULL );

Or you can write it in the same line :

    CREATE TABLE students (roll BIGSERIAL NOT NULL PRIMARY KEY, first_name VARCHAR(50) NOT NULL, last_name VARCHAR(50) NOT NULL, gender VARCHAR(8) NOT NULL, parent_contact VARCHAR(50), department VARCHAR(50) NOT NULL ) ;

We can use `\d` to see all the tables we hava in our database and we can write `\d table_name` to see the details of the mentioned table.

![create_table_students](https://github.com/mechano59/PostgreSQL/blob/master/images/create_table_students.png)

To delete a table we can use the following command :

    DROP TABLE dummy;

**Please note**  : Be very careful while using it because it will delete the table without any further confirmation. So one wrong step and you will lose year's worth of hard work. We can also delete a database by using the `DROP` command like ```DROP DATABASE general;``` 


![drop-database](https://github.com/mechano59/PostgreSQL/blob/master/images/drop_database.png)

In order to insert data in our new table, we will use : 

    INSERT INTO students (
    first_name, 
    last_name, 
    gender, 
    parent_contact, 
    department) 
    VALUES ('Sakib', 'Ahmed', 'Female', '95631780', 'Accounting');

Or,  you can just write it in the same line.

    INSERT INTO students (first_name, last_name, gender, parent_contact, department) values ('Amena', 'Akhter', 'Male', '7282547767', 'Sales Management');

![insert data in table](https://github.com/mechano59/PostgreSQL/blob/master/images/insert_data_in_table.png)

### Viewing the Same Table Differently 

To view all the data in the table at once, use :

    SELECT * FROM students;
And if you want to view a specific column, use :

    SELECT first_name FROM students;
and if you want to arrange the data in certain order, use ORDER BY column_name ASC or DESC : 

    SELECT first_name FROM students ORDER BY first_name ASC;
Or 

    SELECT first_name, gender FROM students ORDER BY first_name DESC;


![asc_desc](https://github.com/mechano59/PostgreSQL/blob/master/images/asc_desc.png)

You can also use AND and OR separately to select Data : 

    SELECT * FROM students WHERE last_name='Begum' AND department= 'Engineering';

Using AND and OR together to select Data : 

    SELECT * FROM students WHERE last_name='Begum' AND ( department= 'Engineering' OR department= 'Human Resources');

![and_or](https://github.com/mechano59/PostgreSQL/blob/master/images/and_or.png)

If you have multiple instances of a data and you want to view just one of its type then you can use `DISTINCT` to do that. 

    SELECT DISTINCT department FROM students ORDER BY department;


![select _distinct](https://github.com/mechano59/PostgreSQL/blob/master/images/select_distinct.png)

### Comparison Operators
 
Although these might seem unimportant or trivial at first, but with a little bit of creativity these commands can make life much easier. 

### Selecting Using Offset and Limit

These can be helpful when one needs to select some specified data from the table within a certain range of serial number. 

`SELECT * FROM students LIMIT 4;`

`SELECT * FROM students OFFSET 2 LIMIT 4;`

![offset_limit](https://github.com/mechano59/PostgreSQL/blob/master/images/offset_limit.png)

### Selecting Multiple Table Data

`SELECT * FROM students WHERE last_name IN ('Hossain', 'Akhter');`

`SELECT * FROM students WHERE last_name IN ('Hossain', 'Akhter') OR department= 'Accounting';`

`SELECT * FROM students WHERE last_name IN ('Hossain', 'Akhter') OR department= 'Accounting' ORDER BY last_name;`


![in_where](https://github.com/mechano59/PostgreSQL/blob/master/images/in_where.png)


Now this is just the tip of the ice-berg. PostgreSQL is a vast topic with immense scope of expanding one's knowledge. What i did is just to give you a head start. Now it's up to you to continue to this path.

## Helpful Commands

These are some of the helpful commands which will get you started smoothly.

To see which Role and Database are you using :

plaban=#`\conninfo`

To see the list of Database :

plaban=#`\l`

To see the list of Roles : 

plaban=#`\du`

To change the Database you are using

plaban=#`\c database_name`

If you feel like sleeping : 

plaban=#`\q`

## Off You Go 
Well there's not much left to teach you now. From here you just need venture on your own "To infinity and beyond." 
there are great tutorials blogs about PostgreSQL which will help you along the way. Also you can always refer to the [PostgreSQL](https://www.postgresql.org/docs/10/app-psql.html)  website for further help. 

Good luck on your journey!



