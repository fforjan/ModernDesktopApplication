***[https://en.wikipedia.org/wiki/microservices](https://en.wikipedia.org/wiki/microservices)***
> In software engineering, a microservice architecture is a variant of the service-oriented architecture structural style. It is an architectural pattern that arranges an application as a collection of loosely coupled, fine-grained services, communicating through lightweight protocols. One of its goals is that teams can develop and deploy their services independently of others. This is achieved by the reduction of several dependencies in the code base, allowing developers to evolve their services with limited restrictions from users, and for additional complexity to be hidden from users.

## From web services
This is also related to [continuous deployment-channels](continuous-deployment-channels.md), the update is managed internally and any rollback can be done easily.

## Extensions approach
Large modern applications are using the extension approach, meaning they have a strong code base and lots of extensions available.

There are two different models available:
- Extensions but delivered as one 'installer': Visual Studio
- Extensions to be installed manually by user via a store: Visual Studio Code

### Extension as one installer
No need for extension management features; the application is delivered as an MSI.
The drawback is that updating one extension requires rebuilding & delivering everything.

### Extension from a store
_Note: there is no off-the-shelf feature available on the market; developers would have to develop it from scratch._
