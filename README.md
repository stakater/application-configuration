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

### Accessing Configuration

The most efficient way to manage configuration is to have a central service through which every application can get the configuration it needs. 

### Modeling Configuration

Each configuration setting can be modeled as a tuple, so the configuration for an application consists of a set of tuples. However, the set of the tuples available and their values typically depend on three things:

- The application
- The version of the application
- The environment it runs in (for example, development, UAT, performance, staging, or production)

Here are some use cases to consider when modeling configuration information:

- Adding a new environment (a new developer workstation perhaps, or a capacity testing environment). In this case you’d need to be able to specify a new set of values for applications deployed into this new environment.

- Creating a new version of the application. Often, this will introduce new configuration settings and get rid of some old ones. You should ensure that when you deploy this new version to production, it can get its new settings, but if you have to roll back to an older version it will use the old ones.

- Promoting a new version of your application from one environment to another. You should ensure that any new settings are available in the new environment, but that the appropriate values are set for this new environment.

- Relocating a database server. You should be able to update, very simply, every configuration setting that references this database to make it point to the new one.

### Testing System Configuration

In the same way that your application and build scripts need testing, so do your configuration settings. There are two parts to testing configuration.

The first stage is to ensure that references to external services in your configu- ration settings are good. 

The second stage is to actually run some smoke tests once your application is installed to make sure it is operating as expected. 

## Managing Configuration across Applications

One of the most important tasks is to keep a catalogue of all the configuration options that each of your applications has, where they are stored, what their lifecycle is, and how they can be changed.

If possible, such information should be generated automatically from each application’s code as part of the build process.

It is especially important to have access to this information on a real-time basis when your applications depend on each other and deployments must be orches- trated.

Configuration management of every application should be planned as part of project inception.

## Principles of Managing Application Configuration

Treat your application’s configuration the same way you treat your code. Manage it properly, and test it. Here is a list of principles to consider when creating an application configuration system:

- Consider where in your application’s lifecycle it makes sense to inject a particular piece of configuration—at the point of assembly where you are packaging your release candidate, at deployment or installation time, at startup time, or at run time. Speak to the operations and support team to work out what their needs are.

- Keep the available configuration options for your application in the same repository as its source code, but keep the values somewhere else. Configuration settings have a lifecycle completely different from that of code, while passwords and other sensitive information should not be checked in to version control at all.

- Configuration should always be performed by automated processes using values taken from your configuration repository, so that you can always identify the configuration of every application in every environment.

- Your configuration system should be able to provide different values to your application (including its packaging, installation, and deployment scripts) based on the application, its version, and the environment it is being deployed into. It should be easy for anyone to see what configuration options are available for a particular version of an application across all environments it will be deployed into.

- Use clear naming conventions for your configuration options. Avoid obscure or cryptic names. Try to imagine someone reading the configuration file without a manual—it should be possible to understand what the configuration properties are.

- Ensure that your configuration information is modular and encapsulated so that changes in one place don’t have knock-on effects for other, apparently unrelated, pieces of configuration.

- Use the DRY (don’t repeat yourself) principle. Define the elements of your configuration so that each concept has only one representation in the set of configuration information.

- Be minimalist: Keep the configuration information as simple and as focused as possible. Avoid creating configuration options except where there is a requirement or where it makes sense to do so.

- Avoid overengineering the configuration system. Keep it as simple as you can.

- Ensure that you have tests for your configuration that are run at deployment or installation time. Check that the services your application depends upon are available, and use smoke tests to assert that any functionality depending on your configuration settings works as it should.

## Conclusion

If your configuration management process is sound, you should be able to answer “yes” to the following questions:

- Could you completely re-create your production system, excluding produc- tion data, from scratch from the version-controlled assets that you store?
- Could you regress to an earlier, known good state of your application?
- Can you be sure that each deployed environment in production, in staging, and in test is set up in precisely the same way?
