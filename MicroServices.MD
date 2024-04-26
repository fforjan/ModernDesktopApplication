***[https://en.wikipedia.org/wiki/Microservices](https://en.wikipedia.org/wiki/Microservices)***
> In software engineering, a microservice architecture is a variant of the service-oriented architecture structural style. It is an architectural pattern that arranges an application as a collection of loosely coupled, fine-grained services, communicating through lightweight protocols. One of its goals is that teams can develop and deploy their services independently of others. This is achieved by the reduction of several dependencies in the code base, allowing developers to evolve their services with limited restrictions from users, and for additional complexity to be hidden from users.

## From web services
this is also related to [Continuous Deployment](ContinuousDeployment.MD), the update is managed internally and any rollback can be done easily.

## Extensions approach.
Large modern application are using extension approach, meaning they have a strong code base and lot of extensions available.

There is two different models available:
- extensions but delivered as one 'installer' : Visual Studio
- extensions to be installed manually by user via a store : Visual Studio Code


### Extension as one installer
No need to extension management features, the application is delivered as a msi.
The drawback is that updating one component - one extensions requires to rebuild & deliver everything


### Extension from a store
___Note: there is no off the shelve such feature avaible on the market, developer would have to develop it from scratch.___


