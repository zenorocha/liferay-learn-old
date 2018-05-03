---
title: "Exercise: Your First Service"
description: "Let's get started!"
icon: "flash"
layout: "guide"
weight: 1
---

###### {$page.description}

<article id="1">

## Install Liferay Developer Tools

In order to follow the exercise in the video, you’ll need to install the following:

+ [Liferay Developer Studio](https://web.liferay.com/get-ee-trial/downloads/developer-studio)
+ [Liferay Workspace](https://customer.liferay.com/documentation/7.0/develop/tutorials/-/official_documentation/tutorials/installing-liferay-workspace)

<aside>

###### <span class="icon-16-star"></span> Pro Tip

If this exercise text leaves questions open, the next chapter contains a step-by-step walkthrough video of installing Liferay Developer Studio and Liferay Workspace, as well as the exercise itself. Feel free to watch it during the exercise or if you run into problems.

</aside>

</article>

<article id="2">

## Exercise Overview

For the first demonstration, we’ll create three projects:

+ a `simple` API for a service
+ a service implementation
+ a client to call the service

As we want to be as simple as possible, we’ll stick with the most basic components. Liferay comes with a shell, called `Gogo Shell`. We’ll display our output there.

</article>

<article id="3">

## Service API

To start, we’ll create a simple API

###### Create

+ `Open` Liferay Deveoper Studio
+ Create a `Liferay Module Project`
+ Name the project: `“helloworld-api”`
+ For `Project Template Name`, select `“api”` from the drop down

###### Follow

+ Click `Next`
+ Component Class Name: `HelloService`
+ a simple API for a service
+ Package Name: `com.liferay.university.hello.api`
+ Click `Finish`

###### Type

+ Open `HelloService.java`
+ Add the `hello` method signature below

```text/x-java
package com.liferay.university.hello.api;

public interface HelloService {
    String hello(String parameter);
}
```

If you use Liferay Developer Studio with Liferay Workspace, it will have a proper project structure. Inspect `bnd.bnd` - it will have the following content:

```text
Bundle-Name: helloworld-api
Bundle-SymbolicName: com.liferay.university.hello.api
Bundle-Version: 1.0.0
Export-Package: com.liferay.university.hello.api
```

###### Deploy To Your Container

When this project is built, we’ll have a bundle that we can deploy to any OSGi container. As you have Liferay, let’s start the server.

+ Open a telnet client on localhost, port 11311, to access Gogo Shell
+ Type g! lb helloworld to list all the bundles (lb) that have “helloworld” in their name

Verify that helloworld-api is Active:

```text
g! lb helloworld
START LEVEL 20
   ID|State  	|Level|Name
  590|Active 	|    1|helloworld-api (1.0.0)
```

</article>

<article id="4">

## Service Implementation

###### Create
Now create a second project with the service implementation.

+ Create a new `service` project in Liferay Developer Studio.
+ Name the project `helloworld-service`

When the project is created, take a look at the `HelloServiceImpl` class:

```text/x-java
package com.liferay.university.hello.impl;

import com.liferay.university.hello.api.HelloService;

import org.osgi.service.component.annotations.Component;

@Component
public class HelloServiceImpl implements HelloService {
    @Override
    public String hello(String parameter) {
   	 return parameter;
    }
}
```

Notice that the class “HelloService” is showing up as unresolved. To fix this, let’s import our interface.

The gradle implementation in Liferay Workspace makes other modules from the same workspace easily available.

+ Open build.gradle
+ Add the following line to the existing dependencies:

```text
compileOnly project(":modules:helloworld-api")
```
+ Next, we need to get gradle to pick up the changed dependencies.
+ Right-click and choose “Gradle/Refresh Gradle Project”

The resulting project will automatically deploy to Liferay, ending up with both of our projects being available:

```text
g! lb helloworld
START LEVEL 20
   ID|State  	|Level|Name
  590|Active 	|	1|helloworld-api (1.0.0)
  591|Active 	|	1|helloworld-service (1.0.0)
```

</article>

<article id="5">

## Calling The Service

To call the service, let’s build a quick and dirty Gogo-Shell command that utilizes our service:

+ Create another project of type “service”
+ Name the project “helloworld-command”

**Note:** This bundle will also depend on helloworld-api, just like the service implementation. Add the same dependency as above to build.gradle.Next, let’s call the relevant HelloService implementation and display the results in Gogo Shell.

```text/x-java
package com.liferay.university.command;

import com.liferay.university.hello.api.HelloService;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component(
   	   immediate=true,
   	   service = Object.class,
   	   property = {
   	 	"osgi.command.function=say",
   	 	"osgi.command.scope=custom"
   	   }
   	 )
public class HelloWorldCommand {
    
    public void say(String what) {
   	 System.out.println(helloService.hello(what));
    }
    
    @Reference    
    private HelloService helloService;
}
```

The @Component declaration will make sure that we can easily use this class as a command in Gogo Shell. Let’s try this: On Gogo Shell, validate that your service is deployed and active:

Let’s try this: Validate that your service is deployed and active in Gogo Shell:


```text
g! lb helloworld
START LEVEL 20
   ID|State  	|Level|Name
  590|Active 	|	1|helloworld-api (1.0.0)
  591|Active 	|	1|helloworld-service (1.0.0)
  592|Active 	|	1|helloworld-command (1.0.0)
```

Now type:

```text
g! say hello
hello
```

**Congratulations,** your first and simplest possible OSGi Declarative Service.

</article>

<article id="6">

## Dependency Injection Through OSGi

Let’s use this simple code for further experimentation with Gogo Shell and mess with the runtime. 
Note: Replace “591” with the ID for your service from Gogo Shell.

```
g! stop 591
g! say hello
gogo: CommandNotFoundException: Command not found: say
```
###### What Happened?

```
g! lb helloworld
START LEVEL 20
   ID|State  	|Level|Name
  590|Active 	|	1|helloworld-api (1.0.0)
  591|Resolved 	|	1|helloworld-service (1.0.0)
  592|Resolved 	|	1|helloworld-command (1.0.0)
```

###### Again: What Happened?

The helloworld-command service has a dependency on “helloworld-service” that is no longer satisfied. Thus, the OSGi runtime has not only stopped the service implementation, but also the helloworld-command service. 

**Next,** start the helloworld-service bundle again and see if helloworld-command is restarted as well.

##### OSGi Bundle Lifecycle

This brings us to the lifecycle of an OSGi bundle. As soon as you have deployed a bundle into an OSGi runtime, the runtime will attempt to resolve all available dependencies:

![alt img](../../images/osgi-basics/OSGi_Bundle_Cycle.png)

Let’s keep things simple with this first exercise, and make it more interesting in the next exercise. We’ll introduce a second implementation for our API and see if the new deployment meets your expectations.

</article>
