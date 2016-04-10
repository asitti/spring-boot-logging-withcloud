# spring-boot-logging-withcloud

This project demonstrates issue(s) found with including Spring Cloud dependency on a Spring Boot
application utilizing a custom _logback-spring.xml_ with springProperty initialization.

It appears the the introduction of the Spring Cloud dependency modifies the lifecycle in such a way
that the logback config is initialized multiple times with potentially different values (or null).

Cases:

## No bootstrap.yml

Running the application yields 2 log files:

- logfile_IS_UNDEFINED.log
- myapp1-application-default.log

## With bootstrap.yml provided (default profile)

Running the application yields 2 log files:

- myapp1-bootstrap-default.log
- myapp1-application-default.log

## With bootstrap.yml provided (test profile)

- myapp1-bootstrap-test.log
- myapp1-application-test.log

# RESOLUTION

When adding the cloud dependency, the custom properties for logback-spring need to be moved to the bootstrap.yml (and
their associated profiles).  From that point, the logging is only initialized once with the correct property.  See 
https://github.com/spring-cloud/spring-cloud-commons/issues/107 for more info.