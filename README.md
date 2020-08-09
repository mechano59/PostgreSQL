# PostgreSQL
![postgresql_logo](https://miro.medium.com/max/800/1*PY24xlr4TpOkXW04HUoqrQ.jpeg)

## Table of Contents

 - [What Is Postgresql](https://github.com/mechano59/PostgreSQL/blob/master/README.md#what-is-postgresql)
 - [Prerequisite](https://github.com/mechano59/PostgreSQL/blob/master/README.md#prerequisite)
 - [Installing PostgreSQL](https://github.com/mechano59/PostgreSQL/blob/master/README.md#installing-postgresql)
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

It's not recommended practice to use the default postgres role for everything. 
So now let's make a new role. For that use : 

    sudo -u postgres createuser --interactive --password
Enter name of role to add: `plaban`

Shall the new role be a superuser? (y/n) : `y`

Password: `123456`  
***please use something stronger.***

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


And its done. you can now easily access using that role by typing :

    sudo -i -u plaban psql

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
