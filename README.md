# Auto refresh Properties on Git PUSH
spring cloud Configuration server,Eureka in Peer Mode,Zuul gateway
## Architecture Diagram

![ArchitechureDiagram](https://user-images.githubusercontent.com/45845757/59155377-c4f2e380-8aa5-11e9-90a9-d50bd9521e04.jpg)

# 2.	Dependency  Required (Config Server)
  <pre>
  spring-cloud-config-serve
  spring-cloud-config-monitor
  spring-cloud-starter-stream-rabbit
  </pre>

# 3.	Dependency  Required (Config client)
<pre>
  spring-cloud-starter-config
  spring-cloud-starter-bus-amqp
  spring-boot-starter-actuator
</pre>

# 4.	Important Setting
<pre>
First create account on ngrok website so that we can test our changes on localhost.
Ngrok will provide local tunnelling feature so that our config server 
will receive push notification from GitHub WebHook. Then download ngrock from https://ngrok.com
Place ngrok.exe any drive other than windows home drive. 
Open command prompt navigate to ngrok exe path and run ngrok http localhost:&lt;config server port>, 
after few second you will receive a link  http://7771d1a8.ngrok.io -> http://localhost:&lt;config server port>. 
Login to your git  account and add webhook URL as http://7771d1a8.ngrok.io/monitor ,
Once save GIT will send ping event to our application which will fail.
Below configuration required to enable actuator endpoint(config client) as of springBoot 2.0

management.endpoint.refresh.enabled=true
management.endpoint.bus-env.enabled=true
management.endpoint.bus-refresh.enabled=true
management.endpoints.web.exposure.include  - refresh
                                         	 - bus-refresh
                                         	 - bus-env

Application refresh strategy is based on application name
Below config file kept on config repo
XXX_SERVICE.yml
YYY_SERVICE.yml
Application.yml

Any changes to application.yml will trigger all application refresh event.
Any changes to XXX_ or YYY_ will trigger specific application refresh event.
</pre>
