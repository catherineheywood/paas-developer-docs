To be hosted by Government PaaS, your application must:

* follow the [Twelve-Factor application](http://12factor.net/) principles
* be written in a language supported by the [default Cloud Foundry buildpacks](http://docs.cloudfoundry.org/buildpacks/) eg, Go, Nodejs, Java, Python, PHP, Python, Ruby (we don’t support custom buildpacks right now)
* not carry data at SECRET or above (this is currently out of scope for PaaS)

### 12-Factor application principles
Heroku developers designed 12 best practices for modern apps to follow during development to make them scalable and easy to deploy. 

We have also summarised these practices in the table below, and noted the relevance of each principle to the Government PaaS running Cloud Foundry. 

Visit the [12factor.net website](http://12factor.net/) to further ensure you application supports these practices.

|Principle  |Meaning  |Relevance to Cloud Foundry  |
|:---|:---|:---|
|One codebase many deploys  |Each of your applications needs its own source-controlled repository.  |You can push the same codebase to many Cloud Foundry applications with these commands: ``` $ cf push myapp-staging``` <br/> ```$ cf push myapp-production```  |
|Isolate dependencies |All required dependencies (eg a database or image library) must be vendored into your software system.|If you don’t declare your dependencies, you probably won’t be able to deploy your application on Cloud Foundry. How you specify dependencies depends on which language and buildpack you use. For example, the python buildback expects you to provide a requirements.txt file which it will pass to pip.  |
|Store your configuration in the environment  | Ensure you separate the storage of your code and configuration (this is anything that may vary between environments, eg passwords). |Cloud Foundry provides environment variables to tell an application how to configure itself, eg VCAP _SERVICES tells applications what services are available and how to connect to them.  |
|Backing Services  |All your backing services (eg an email service or a monitoring system) should be loosely coupled to your code so you can easily change services if you need to. A change in service should not require a code change.|In Cloud Foundry, backing services are referred to as ‘services’. Users can create services, bind them to applications, and delete them.  |
|Strictly separate build, release and run stages  |There should be a strict separation between building, releasing and running the code.  |The build stage is carried out by the buildpack and Cloud Foundry refers to this as ‘staging’ an app. Cloud Foundry combines the release and run stage into one stage. |
|Stateless processes  |Build stateless applications where intermediate data is stored in your backing services (eg your databases) and not in your running code. Also, have you applications run on many servers so you can ensure continuity in the event of server downtime.  | Cloud Foundry treats all application processes are stateless and expendable.|
|Port binding  |Your application should interface to the world via an API. You can have a separate URL for your customers than the one you use for internal calls. This means your app can be used as a backing service for another app via its URL.  | Cloud Foundry expects applications to have this and provides an environment variable called PORT for the application to use as part of it's bootstrapping process |
|Concurrency  | Ensure all your processes (eg web requests or API calls) are running separately so your application can scale easily.  | Cloud Foundry expects applications to behave according to this principle. To increase the number of processes running, use the ```cf scale``` command.|
|Disposability  | You should be able to release new code rapidly. Also, applications should be able to start back up fast and cleanly following shut down.  | Cloud Foundry expects applications to follow this principle.|
|Development parity  | Applications should be rapidly deployed from their development environment to production.  To ensure rapid deployment, keep the developer environment similar to that of production (eg both environments should use the same backing services).  | CF helps you achieve this development/production parity by letting you create and deploy similar services for development, test and production environments|
|Logs  | Maintain and archive log files so you have visibility of how your application works over time.  |Cloud Foundry [describes how to log files here](https://docs.cloudfoundry.org/devguide/deploy-apps/streaming-logs.html). PaaS will offer the Loggregator function in beta. |
|Administrative processes  |Run one-off administrative processes (eg running analytics) in your production environment.  |  |
