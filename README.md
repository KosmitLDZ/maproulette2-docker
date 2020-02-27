# MapRoulette in Docker
Docker image and deployment scripts for Map Roulette api, database and fronted. This tool creates a MapRoulette backend and frontend images as well as a postgres-postgis image. The postgis image is using [https://hub.docker.com/r/mdillon/postgis/](mdillon/postgis). A default database is created mrdata and the container is linked to the MapRoulette API container. This can be used for deploying the entire stack or any of the 3 components individually.

# Deployment

### Setting up Configuration

##### API 
There are a couple of required properties that you will need to setup prior to running the docker-compose commands.

* **db.default.url** - This is the location of the database. This is set to the linked postgres docker container, so doesn't need to be explicitly set unless using a different database. If you are not deploying the Postgres database using Docker then update this field with the connection string to your external database
* **osm.consumerKey** - This is the consumer key for your MapRoulette application in your openstreetmap.org account. 
* **osm.consumerSecret** - This is the consumer secret that is found in the same MapRoulette application settings as is the consumer key.
* **maproulette.super.key** - This is the api key that can be used for any API requests and the server will assume that the person making the request is a super user. No requirement to login.
* **maproulette.super.accounts** - This is a comma separated list of OSM account ids that will be elevated to super user access when they login.  

##### Frontend
The frontend requires certain properties to be updated as well.

* **REACT_APP_BASE_PATH** - This is the root path for the MapRoulette frontend App. Which by default is "/" and wouldn't ordinarily need to be changed.
* **REACT_APP_URL** - This is the root url for the MapRoulette frontend App. By default it is localhost:3000, the reason for this is that it is generally advisable to front these services with a http server like nginx or Apache webserver. And those web servers would then just proxy requests on port 80 to port 3000. But if you don't need certain features you can change this to port 80 instead and not have it fronted by a web server.
* **REACT_APP_MAP_ROULETTE_SERVER_URL** - This is the root server url for the MapRoulette backend. By default the MapRoulette backend will startup on port 9000, but if you are pointing to an instance that has started up on a different port you can change that here.

### Build and run

To build images required for correct MapRoulette release you need to specify **gitTag** variable for each frontend and backend images. If left **LATEST** it will fetch latest master branches for each repository.<br /><br />
Build command:
```
docker-compose build
```
Run command:
```
docker-compose up
```


### FAQ

**I want to connect to my own database, how do I do that?**

This is fortunately very easy to accomplish. You can specify your database connection info as environment variables for backend image.
