# Session Management using Spring Session Redis in Spring Boot

## Enable Redis Session management
Just by adding `spring-boot-starter-data-redis` in you pom.xml
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

`docker run --name <some-redis> -d redis` it's recommended naming your container

#### Run the example
- Start the application and goto the [hello-page](http://localhost:8080/hello)
- Login with `user` and `generated-security-password` (logging terminal)
- Now when you restart the application and refresh the page, there is no need to login again.

#### Test if Redis stores your session
- Try the application without `spring-boot-starter-data-redis`, now you need to re-login after restarting the application.
- Check `JSESSIONID` and/or `SESSION` cookies via your browser devtools (F12)
- Check generated `keys *` via `redis cli`

#### Redis CLI via terminal in Docker Desktop
- Open the Terminal-tab and type `redis-cli`
- Use command `keys *` to inspect the generated keys.
  There should be some `spring:session...` keys listed

#### Run Redis CLI via cmd
- Create a <i>redis-</i>network: `docker network create redis-network`
- `docker run -it --network redis-network --rm redis redis-cli -h redis` 