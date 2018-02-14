- Install docker - Just download and install.

- Install dns masq:

    Important: Don't use the echo from the link, use the echo from this file, because it adds
    dt7.vt-s.net to the dnsmasq. Other settings you should follow up, and after pasting the echo command bellow,
    you should ping those to see if it has response.

    configure dns masq: https://gitlab.com/MacMladen/mci-drupal/blob/master/docs/resolving.md#mac

    echo "
    ### local development ###
    address=/dev/127.0.0.1
    address=/dt7.vt-s.net/127.0.0.1
    address=/loc/127.0.0.1
    listen-address=127.0.0.1
    " >> /usr/local/etc/dnsmasq.conf

- Important: add alias to the resolver

    echo "nameserver 127.0.0.1" | sudo tee /etc/resolver/dt7.vt-s.net

    You can check if it's created by going to /etc/resolver

- In the home directory, create a directory - Sites
    We will use this directory to hold our websites, and to hold the traefik directory.

    mkdir Sites

- Enter directory, and clone the vts-docker repository. With a dot (.)!

    cd Sites

    git clone https://github.com/technivant/vtsdocker.git .

- After the clone, go to traefik directory.

    docker-compose up -d

    This will bring up the traefik and portainer containers, which are in charge of traffic and ports.
    You can check those in the browser by typing traefik.loc and portainer.loc


- Set up directories for websites

    In the Sites directory, next to the traefik directory, we can add as much website directories needed.
    The core of the docker environment setup is the docker-compose file, and the www directory which should
    hold the drupal root. Docker-compose file points to the www directory.

    If we don't have the eliotme directory, or any other website, we just make a directory with the site name,
    and in it we make a directory named www. We can copy the docker-compose file from other websites, and just change the
    aliases in the docker-compose.

    Important: The docker-compose.yml from the traefik directory is different from the docker-compose.yml from the website directory.
    If you copy the one from traefik it won't have the services needed for the website.

    mkdir eliotme
    cd eliotme
    mkdir www

    And in www directory we clone our t7 repository.


- Bring up the containers - This is done in terminal where docker-compose.yml is located, in our case, in the eliotme directory,
 and not in the www because it contains drupal files, and not the docker-compose file.

    docker-compose up -d

- After that, we can do 'docker ps' in the terminal, and we will see the list of all the containers which have been created.

- Configuring website - database, settings.php

    Now we have created the website directory, brought up the containers, now our work is in eliotme/www/sites/eliotme

    Or in any other case [site_name]/www/sites/[site_name]/settings.php

    If it's an existing website, we copy the settings.php from this directory to www/sites/eliotme/settings.php ,
    and then we import the database with the php-my-admin, in our case it will have the alias (we can see aliases in traefic.loc or in the websites docker-compose file)

    http://eliotme.pma.loc

    There we create the drupal database and import it.

    If it's a new website, the process is the same, the only difference is we have to create a directory which will hold the vts theme and then add a settings.php file.

- Configuring SOLR

    docker-compose exec solr make init -f /usr/local/bin/actions.mk create core=drupal

    This will make a drupal core with the config's needed, and then we have to set it up through the admin UI

    Go to: /admin/config/search/search_api/server/solr/edit  and configure the solr server:

        - Solr host: solr
        - Port: 8983
        - Core name: solr/drupal    - In this drupal is the name of our core, it can be solr/core_1 or whatever you make it.

- Configuring Varnish:

    -Varnish version
    4

    -Varnish Control Terminal:
    varnish:6082

    -Varnish Control Key
    secret



- Configuring Redis

    -Localhost:
    redis

    -Client:
    none or automatic

-Clamav

    -Hostname
    clamav





- Check the broswer for the running website

    Go to web browser, and type in http://eliotme.dt7.vt-s.net/ for the website.

    Type in eliotme.pma.loc - For PHP MY ADMIN panel - we can import/export databases there.

    Type in http://eliotme.hog.loc/ for mail hog.

    If you're redirected to google, you're missing the http:// before the adress.





