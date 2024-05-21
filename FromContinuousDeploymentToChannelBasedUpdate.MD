[https://en.wikipedia.org/wiki/Continuous_deployment](https://en.wikipedia.org/wiki/Continuous_deployment)
>Continuous deployment (CD) is a software engineering approach in which software functionalities are delivered frequently and through automated deployments. Continuous deployment contrasts with continuous delivery (also abbreviated CD), a similar approach in which software functionalities are also frequently delivered and deemed to be potentially capable of being deployed, but are actually not deployed.[4] As such, continuous deployment can be viewed as a more complete form of automation than continuous delivery

Multiple channel
- release
- preview
- daily

Available solution:
1. dotnet tool deployment
no channel, just lastest available.
no bandwith issue, hosted by nuget.org ?

1. https://github.com/Squirrel/Squirrel.Windows/

Note: in case of extensions based architecture, when using preview or daily build, internal component may be updated during a preview cycle, either by replace the full componenet or black listing some of the service.
no  bandwith issue, hosted by nuget.org ?
