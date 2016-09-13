# Station 4 Spring Cloud Services

## Introduction
Spring Cloud Services is a seriese of tools to help build microservices style applications in Spring.  They include several of the components from the NetFlixOSS components all wrapped up and easy to use for a Spring developer.  Pivotal Cloud Foundry also provides the runtime components as pre-packaged services that are easy to use.  In this exercise you will create two microservices one that calls another one.  However we'll make sure that call is protected by a circuit breaker.   For more of an explaination of the Circuit Breaker pattern ask the person helping with this station.


##Setup
Start by making a directory and changing into it

```
mkdir springcloud
cd springcloud
```

##Review the Circuit Breaker Pattern
A great description and diagram showing the Circut Breaker pattern is at:https://docs.pivotal.io/spring-cloud-services/circuit-breaker


**download the project**
```
git clone https://github.com/spring-cloud-samples/traveler
```

Change into the traveller directory
```
cd traveler
```

Build the app
```
mvn package
```

**login to PCF**  
**TBD**
- [ ] need to provision service-registry and circuit-breaker-dashboard into the cloudbrews space
```
cf login -a api.run.pivotal.io -u demo4@johnfunk.com -o Channel -s Denver-CloudBrews
```
Agency App
```
cd agency
cf push agency-cloudbrews -p target/agency-0.0.1-SNAPSHOT.jar
```
**TBD**  
- [ ] need to make sure the routes don't conflict maybee use --random-route??

Company App
```
cd ..
cd company
cf push company-cloudbrews -p target/company-0.0.1-SNAPSHOT.jar
```
**TBD**
- [ ] need to make sure the routes don't conflict maybee use --random-route??

**Agency App**
http://agency-cloudbrews.cfapps.io/

***Company App***
https://company-cloudbrews.cfapps.io/available

**Continuosly Poll the app using Curl**
```
while true; do curl http://agency-cloudbrews.cfapps.io/ ;done
```

**Look at the Dashboard**
https://hystrix-eb12265a-15ef-4ae7-b022-1fa5cb52d9ed.cfapps.io/hystrix/monitor?stream=https%3A%2F%2Fturbine-eb12265a-15ef-4ae7-b022-1fa5cb52d9ed.cfapps.io%2Fturbine.stream

**Stop the Company App**
You will notice the Agency App continues to run taking the default path
```
cf stop company
```

**Re-Start the Company App**
You will notice the Agency App continues returns to calling the company app 
```
cf start company
```
