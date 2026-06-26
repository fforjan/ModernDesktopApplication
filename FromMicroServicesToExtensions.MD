***[https://en.wikipedia.org/wiki/Microservices](https://en.wikipedia.org/wiki/Microservices)***
> In software engineering, a microservice architecture is a variant of the service-oriented architecture structural style. It is an architectural pattern that arranges an application as a collection of loosely coupled, fine-grained services, communicating through lightweight protocols. One of its goals is that teams can develop and deploy their services independently of others. This is achieved by the reduction of several dependencies in the code base, allowing developers to evolve their services with limited restrictions from users, and for additional complexity to be hidden from users.

## From web services

this is also related to [Continuous Deployment](FromContinuousDeploymentToChannelBasedUpdate.MD), the update is managed internally and any rollback can be done easily.

## Extensions approach

Large modern application are using extension approach, meaning they have a strong code base and lot of extensions available.

There are two different models available:

- extensions but delivered as one 'installer' : Visual Studio
- extensions to be installed manually by user via a store : Visual Studio Code

### Extension as one installer

No need to extension management features, the application is delivered as a msi.
The drawback is that updating one component - one extensions requires to rebuild & deliver everything

### Extension from a store

___Note: there is no off the shelve such feature avaible on the market, developer would have to develop it from scratch.___

### Extensions via NuGet.org

**Overview:**
Using NuGet.org as an extension distribution channel leverages the existing .NET package infrastructure.

**How it works:**
- Package each extension as a NuGet package
- Push to NuGet.org (public) or private NuGet feed
- Application loads extensions at runtime from package directory
- Can use package versioning and dependencies

**Advantages:**
- Leverage existing NuGet tooling and infrastructure
- Familiar workflow for .NET developers
- Built-in versioning and dependency management
- Public or private feed options available
- Automated CI/CD integration with package publishing
- Free (NuGet.org) or self-hosted option

**Considerations:**
- Requires extension loading framework (MEF, Roslyn analyzers, or custom reflection)
- Need to implement extension discovery and lifecycle management
- NuGet packages are static binaries (not runtime-editable)
- Extension isolation and security require additional consideration
- Manual extension installation vs automatic updates require orchestration

**Best for:**
- Internal/enterprise extensions (private NuGet feed)
- Applications where developers also build extensions
- Teams already using NuGet for package management
- Extensions that are essentially .NET libraries

## Integration with Channel-Based Updates

When using an extension-based architecture with [channel-based deployment](FromContinuousDeploymentToChannelBasedUpdate.MD):

**Core application:** Can be on stable release channel while users opt into preview or daily builds

**Per-extension versioning:** Each extension can have independent channels (one extension on preview, others on stable)

**Preview cycle management:** During preview cycles, internal components can be updated by:
- Replacing the full component
- Blacklisting certain services to prevent conflicts
- Rolling back specific extensions without affecting core app

**Bandwidth efficiency:** Only changed extensions are downloaded in updates, not the entire application

**User control:** Users can mix stable and preview versions across different extensions
