# application-configuration
what is application configuration?

## Definition

Application configuration allows to customize, tweak or select - without recompilation ! - the features and general behaviour (e.g., performance and security) of an application and the libraries it uses. This covers configuration of *application-speci2c* options as well as options of the *3rd party libraries/frameworks* used by this application (e.g., Log4J or JDBC), but *excludes* configuration of an application's infrastructure (e.g., Docker, Chef or Puppet).

Configuration is one of the three key parts that comprise an application, along with its binaries and its data. Configuration information can be used to change the behavior of software at **build time**, **deploy time**, and **run time**. Delivery teams need to consider carefully what configuration options should be available, how to manage them throughout the application’s life, and how to ensure that configuration is managed consistently across components, applications, and technologies. We believe that you should treat the configuration of your system in the same way you treat your code: Make it subject to proper management and testing.

In our experience, it is an enduring myth that configuration information is somehow less risky to change than source code. Our bet is that, given access to both, we can stop your system at least as easily by changing the configuration as by changing the source code. If we change the source code, there are a variety of ways in which we are protected from ourselves; the compiler will rule out nonsense, and automated tests should catch most other errors. On the other hand, most configuration information is free-form and untested.

**Configurable software is not always the cheaper solution it appears to be. It’s almost always better to focus on delivering the high-value functionality with little configuration and then add configuration options later when necessary.**

Don’t misunderstand us: Configuration is not inherently evil. But it needs to be managed carefully and consistently. 

## Types of Configuration

Configuration information can be injected into your application at several points in your build, deploy, test, and release process, and it’s usual for it to be included at more than one point.

- Your build scripts can pull configuration in and incorporate it into your binaries at **build time**.
- Your packaging software can inject configuration at **packaging time**, such as when creating assemblies, ears, or gems.
- Your deployment scripts or installers can fetch the necessary information or ask the user for it and pass it to your application at **deployment time** as part of the installation process.
- Your application itself can fetch configuration at **startup time** or **run time**

**Generally, we consider it bad practice to inject configuration information at build or packaging time.** This follows from the principle that you should be able to deploy the same binaries to every environment so you can ensure that the thing that you release is the same thing that you tested. The corollary of this is that anything that changes between deployments needs to be captured as configuration, and not baked in when the application is compiled or packaged.

It is usually important to be able to configure your application at deployment time so that you can tell it where the services it depends upon (such as database, messaging servers, or external systems) belong. For example, if the runtime configuration of your application is stored in a database, you may want to pass the database’s connection parameters to the application at deployment time so it can retrieve it when it starts up.

Finally, you may need to configure your application at **startup time** or at **run time**. Startup-time configuration can be supplied either in the form of environment variables or as arguments to the command used to start the system. Alternatively, you can use the same mechanisms that you use for runtime configuration: registry settings, a database, configuration files, or an external configuration service (accessed via SOAP or a REST-style interface, for example).

Whatever mechanism you choose, we strongly recommend that, as far as practically possible, you should try and supply all configuration information for all the applications and environments in your organization through the same mechanism. This isn’t always possible, but when it is, it means that there is a single source of configuration to change, manage, version-control, and override (if necessary). 

## Managing Application Configuration

There are three questions to consider when managing your application’s configuration:

1. How do you represent your configuration information?
2. How do your deployment scripts access it?
3. How does it vary between environments, applications, and versions of applications?

