# Spring Boot 3: Session Management using Spring Session Redis

## Enable Redis Session management
All you need to do is adding `spring-boot-starter-data-redis` in you pom.xml
```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

And providing the redis connection properties in application.yaml (or application.properties)

*example (redis running via docker):*
```yaml
spring:
  redis:
    port: 6379
    host: localhost
```

## Redis with Docker (ref [Redis Docker](https://www.docker.com/blog/how-to-use-the-redis-docker-official-image/))

#### Pull a light-weight Redis image

`docker pull redis:alpine3.17`

#### Run Redis service in “detached” mode

`docker run --name redis-server -d redis:alpine3.17` it's recommended naming your container (here that is 'redis-server')

#### Run the example
- Start the application and goto the [hello-page](http://localhost:8080/hello)
- Login with `user` and `generated-security-password` (logging terminal)
- Now when you restart the application and refresh the page, there is no need to login again.

#### Test if Redis stores your session
- Try the application without `spring-boot-starter-data-redis`, now you need to re-login after restarting the application.
- Check `JSESSIONID` and/or `SESSION` cookies via your browser devtools (F12)
- Check generated `keys *` via `redis cli`

#### Redis CLI via terminal in [Docker Desktop](https://www.docker.com/products/docker-desktop)
- Open the Terminal-tab and type `redis-cli`
- Use command `keys *` to inspect the generated keys.
  There should be some `spring:session...` keys listed

#### Run Redis CLI via cmd
- In cmd use `docker exec -it redis-server redis-cli` to start redis-cli in your container (here 'redis' is the container name).
- The keys `spring:session...` will be listed with command `keys spring:session:*` (or `keys *` to see all generated keys)