(note: [Eugene Ware](http://github.com/eugeneware) has a Docker wordpress container build on nginx with some other goodies; you can check out his work [here](http://github.com/eugeneware/docker-wordpress-nginx).)


(N.B. the way that Docker handles permissions may vary depending on your current Docker version. If you're getting errors like
```
dial unix /var/run/docker.sock: permission denied
```
when you run the below commands, simply use sudo. This is a [known issue](https://twitter.com/docker/status/366040073793323008).)


This repo contains a recipe for making a [Docker](http://docker.io) container for Wordpress, using Linux, Apache and MySQL. 
To build, make sure you have Docker [installed](http://www.docker.io/gettingstarted/), clone this repo somewhere, and then run:
```
docker build -rm -t <yourname>/wordpress .
```

Or, alternatively, build DIRECTLY from the github repo like some sort of AMAZING FUTURO JULES-VERNESQUE SEA EXPLORER:
```
docker build -rm -t <yourname>/wordpress git://github.com/jbfink/docker-wordpress.git
```

Then run it, specifying your desired ports! Woo! 
```
docker run --name wordpress -v <host wp-content>:/var/www/wp-content -e WP_DBNAME=<db name> -d -p <http_port>:80 -p <ssh_port>:22 <yourname>/wordpress 
```
Create in your wp-content a folder named "dump". In this folder put your mysql dump and name it dump.sql. When the container starts up, the dump is loaded in to the mysql wordpress db. Before the container is shutted down, the dump.sql file is updated with the last modify.

The db name env var is used as the name of the db and added to suffix table name in your wp-config.php

```
'wp_$DB_NAME'
```

Check docker logs after running to see MySQL root password and Wordpress MySQL password, as follows

```
echo $(docker logs wordpress | grep password)
```

(note: you won't need the mysql root or the wordpress db password normally)


Your wordpress container should now be live, open a web browser and go to http://localhost:<http_port>, then fill out the form. No need to mess with wp-config.php, it's been auto-generated with proper values. 


Note that this image now has a user account (appropriately named "user") and passwordless sudo for that user account. The password is generated upon startup; check logs for "ssh user password", and something like this to get in: 

```
ssh -p <ssh_port> user@localhost
```

You can shutdown your wordpress container like this:
```
docker stop wordpress
```

And start it back up like this:
```
docker start wordpress
```

Enjoy your wordpress install courtesy of Docker!
