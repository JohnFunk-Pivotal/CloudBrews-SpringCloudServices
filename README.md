# CloudBrews-SpringCloudServices

**Setup**
Start by making a directory and changing into it

```
mkdir springcloud
cd springcloud
```

**Review the Circuit Breaker description and diagram at 
https://docs.pivotal.io/spring-cloud-services/circuit-breaker/**


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
https://spring-cloud-service-broker.cfapps.io/dashboard/p-circuit-breaker-dashboard/c2341566-ce4e-41e5-8602-da7d0ed07415

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
