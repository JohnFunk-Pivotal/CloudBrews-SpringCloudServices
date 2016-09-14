# Station 4 Spring Cloud Services

## Introduction
Spring Cloud Services is a seriese of tools to help build microservices style applications in Spring.  They include several of the components from the NetFlixOSS components all wrapped up and easy to use for a Spring developer.  Pivotal Cloud Foundry also provides the runtime components as pre-packaged services that are easy to use. 

![Spring Cloud Services Diagram](https://github.com/JohnFunk-Pivotal/CloudBrews-SpringCloudServices/blob/master/SpringCloudServices.png "Spring Cloud Services Diagram")

In this exercise you will create two microservices one that calls another one.  However we'll make sure that call is protected by a circuit breaker.   For more of an explaination of the Circuit Breaker pattern ask the person helping with this station.


##Setup
Start by cleaning up after the last person and making a directory in your Google Compute Engine Console and changing into it.

```
cd ~
rm -rf springcloud
mkdir springcloud
cd springcloud
```

##Review the Circuit Breaker Pattern
A great description and diagram showing the Circut Breaker pattern is at:https://docs.pivotal.io/spring-cloud-services/circuit-breaker


## Download the project
```
git clone https://github.com/spring-cloud-samples/traveler
```

## Change into the traveller directory
```
cd traveler
```

## Build the app
```
mvn package
```

##Login to Pivotal Cloud Foundry
```
cf login -a api.run.pivotal.io -u demo4@johnfunk.com -o Channel -s Denver-CloudBrews
```
If you are asked for a password please ask one of the helpers at the station to proide it.

#Push the Agency microservice to Pivotal Cloud Foundry
```
cd agency
cf push agency-cloudbrews -p target/agency-0.0.1-SNAPSHOT.jar
```

#Push the Company microservice to Pivotal Cloud Foundry
```
cd ..
cd company
cf push company-cloudbrews -p target/company-0.0.1-SNAPSHOT.jar
```

## View the output of the Agency microservice
Open the following link in a browser: http://agency-cloudbrews.cfapps.io/

## View the output of the Company microservice
Open the following link in a browser: https://company-cloudbrews.cfapps.io/available

## Continously poll the Agency microservice with curl
```
while true; do curl http://agency-cloudbrews.cfapps.io/ ;done
```

## Look at the Dashboard**
https://hystrix-eb12265a-15ef-4ae7-b022-1fa5cb52d9ed.cfapps.io/hystrix/monitor?stream=https%3A%2F%2Fturbine-eb12265a-15ef-4ae7-b022-1fa5cb52d9ed.cfapps.io%2Fturbine.stream

## Stop the Company microservice
You will notice the Agency App continues to run taking the default path
```
cf stop company
```

##Re-Start the Company App
You will notice the Agency microservice continues returns to calling the company microservice 
```
cf start company
```

## Congradulations
Congratulations you just built a Spring Cloud Services application consisting of two microservices on Google Compute Engine and pushed it to Pivotal Cloud Foundry. Talk to the helper at the station to see what's next!
