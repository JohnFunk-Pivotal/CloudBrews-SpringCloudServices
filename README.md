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
cf login -a api.run.pivotal.io -u jfunk@pivotal.io -o Channel -s Denver-CloudBrews
```
Agency App
```
cd agency
cf push agency-jpf -p target/agency-0.0.1-SNAPSHOT.jar
```
Company App
```
cd ..
cd company
cf push company-jpf -p target/company-0.0.1-SNAPSHOT.jar
```

Agency App
- [ ] need to make sure the routes don't conflict maybee use --random-route??
http://agency-jpf.cfapps.io/

Company App
https://company-jpf.cfapps.io/available

Curl Command:
```
while true; do curl http://agency-jpf.cfapps.io/ ;done
```



== Trying It Out

. Visit `[ROUTE]`, where `[ROUTE]` is the route bound to the Agency application. In the Agency application, the `getGuide()` method on the `TravelAgent` class will use the Service Registry to look up the Company application and get a guide name, which will be randomly selected from a pool of three.
+
image::guide.png[link:docs/images/guide.png]
+
You can simulate load on the Agency application by using `curl`.
+
....
$ while true; do curl http://agency.wise.com; done
....

. To see the circuit breaker in action, stop the Company application. (You can do this either from Pivotal Cloud Foundry Apps Manager or using the cf Command Line Interface tool.)
+
....
$ cf stop company
Stopping app company in org myorg / space development as user...
OK
....
+
The Agency application&#8217;s calls to the Company application will begin failing. Once the failure rate reaches a threshold, the Circuit Breaker Dashboard will trip the `getGuide()` breaker. The calls will fall back to the Agency application&#8217;s own `getBackupGuide()` method and return a static backup guide name while the circuit is open.
+
image::backup-guide.png[link:docs/images/backup-guide.png]

. Check the dashboard to see the breaker status. Alongside the graph, the dashboard shows the status of calls made to `getGuide()` in the last 10 seconds. Call counts are color-coded.
+
image::open-circuit.png[link:docs/images/open-circuit.png]

. Start the Company application again (using either Apps Manager or the cf CLI).
+
....
$ cf start company
Starting app company in org myorg / space development as user...

0 of 1 instances running, 1 starting
0 of 1 instances running, 1 starting
1 of 1 instances running

App started
....
+
After the Company application has started and re-registered with the Service Registry instance and after the registration has trickled down to the Agency application, the Circuit Breaker Dashboard will close the `getGuide()` circuit.
+
image::closed-circuit.png[link:docs/images/closed-circuit.png]
+
You should again see calls to the Company application going through.
