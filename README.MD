![License MIT][1]

### What's this all about?
This is the basis for automating the proxying of Docker web services. It uses a combination of Docker, Registrator, Consul, Consul Template and Nginx to monitor, publish and reverse proxy all running Docker services that have been tagged as "web" services.

### Usage
#### The easy way
If you're happy to make use of Docker Compose, then you're already most of the way there!

Edit the `docker-compose.yml` file and add your service to it. The commented out example service should hopefully be a good reference point.

There are a couple of must haves, but the rest is up to you!

* Two environment variables need to be setup (these are used to name and tag the service in Consul)
  * `SERVICE_NAME=<your-service-name-here>`
  * `SERVICE_TAGS=web[,main]`
* Expose the port the service is hosted on inside the Docker image

The `[,main]` tag is optional, and should only be specified for a single service. The service tagged with this will be hosted at the top end point of your webserver.

All services that are run this way will be hosted on your webserver at the `/<your-service-name-here>/` end point.

```
helloworld-web:
  image: gjon/helloworld-web
  environment:
    - SERVICE_NAME=helloworld
    - SERVICE_TAGS=web,main
  ports:
    - "80"
```

Once that's configured, a call to Docker Compose will get you underway.

```
cd /directory/with/docker-compose/file
sudo docker-compose up -d
```

### Attribution
This didn't all magically come out of my own head, some "smarter than I" individuals have helped me put this together:

* [Scalable Architecture DR CoN: Docker, Registrator, Consul, Consul Template and Nginx][2] by [Graham Jenson][3]

[1]: https://img.shields.io/badge/license-MIT-blue.svg
[2]: http://www.maori.geek.nz/scalable_architecture_dr_con_docker_registrator_consul_nginx/
[3]: http://www.maori.geek.nz/author/graham/