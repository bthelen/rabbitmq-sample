# RabbitMQ Spring Sample for PAS

This project is just a sample, based on the [Spring Guide](https://spring.io/guides/gs/messaging-rabbitmq/) for 
Messaging with RabbitMQ.

This sample changes the original sample somewhat and will send a message every 5 seconds that should then be consumed 
and printed out in the logs.

There is no UI or anything for this sample, it's solely meant to be a way to generate a recurring message to RabbitMQ 
will is also consumed.  The logs will tell you if this mechanism is working properly.

# Environment Parsing

The `master` branch of this repo uses `spring-boot-starter-cloud-connectors` to parse the environment variables and 
configure the RabbitMQ service details for you when running on Cloud Foundry.  The `spring-boot-starter-cloud-connectors` 
has been moved to maintenance release only at this point. The `java-cfenv` branch uses the newer `java-cfenv-boot` 
component to parse the environment variables and configure the RabbitMQ service details for you.  Both branches should 
work identically.
    
# Usage

## Build the Source and Run Locally

### First Run RabbitMQ

Either use docker compose:
```bash
$ docker-compose up
```

Or run locally via your normal means.

### Build the app

```bash
$ ./gradlew build
```

### Run the app locally

```bash
$ ./gradlew bootRun
```

Once the app is running you should see the send and receive messages in your logs:

```
2019-07-03 11:40:08.168  WARN 75827 --- [   scheduling-1] hello.Runner                             : Sending message...
2019-07-03 11:40:08.171  WARN 75827 --- [    container-1] hello.Receiver                           : Received <Hello from RabbitMQ!>
```

## Push the Application to Pivotal Application Service

### Build the app

```bash
$ ./gradlew build
```

### Create a RabbitMQ Service

Use the `cf create-service` command to provision an appropriate service instance for your platform according to the 
plans available.  The `manifest.yml` in this project assumes the service will be called `rabbit` - so please adjust 
the manifest if you choose another name for your service instance.

### Push the App

```bash
$ cf push
```

### View your logs and you should see the send and receive messages:

```bash
$ cf logs -f rabbit-test
```

