---
title: "Deploy"
description: "Let's get started!"
layout: "guide"
icon: "cloud"
weight: 4
---

###### {$page.description}

<article id="1">

## Deploy Multiple Implementations of a Service


Each API can have multiple service implementations. Let’s try it out to see how multiple service implementations interact in the OSGi runtime.


<aside>

###### <span class="icon-16-star"></span> Pro Tip

If you run into problems, or if something in here is unclear, check out the video in the next chapter for a step-by-step walkthrough video of the entire exercise.

</aside>

###### Your Task

Create a new OSGi service that overrides the helloworld API.


```text/x-java
package com.liferay.university.hello.uppercase.impl;

import com.liferay.university.hello.api.HelloService;
import org.osgi.service.component.annotations.Component;

@Component(
    immediate = true,
    service = HelloService.class
)
public class HelloUppercaseServiceImpl implements HelloService {
    @Override
    public String hello(String parameter) {
        return parameter.toUpperCase();
    }
}
```

<aside>

###### <span class="icon-16-star"></span> Pro Tip

Notice that we're using another package here `("com.liferay.university.hello.uppercase.impl")`. OSGi works best when every package is published by exactly one bundle.

</aside>

</article>

<article id="2">

## Before Deployment
Check that your command from the first exercise still works.

```text
g! lb helloworld
g! say hello
hello
```
</article>

<article id="3">

##### Deploy Your New Service Implementation
Next, deploy your new service implementation. Now that you have two implementations for the helloworld API, what do you expect to be printed now?

```text
hello or HELLO
```

</article>

<article id="4">

## Determining Service Implementation Priority
OSGi has the choice to freely bind whatever it needs. The requirement to have a service is satisfied by either of the two implementations. As the service is already bound, there’s no need to rebind.

**Service Bundle ID**

In fact, one of OSGi’s strategies to determine which service has higher priority is the service bundle id - in this case the original implementation: A lower id has higher priority and will be preferred over a higher id.

**But Wait, There’s More**

You might have heard about another criterion, one that you can influence: It’s called service rank. By default, services have a rank of 0, and if you configure a higher rank, the higher ranked service is preferred.

**Note:** For services with the same rank, the OSGi runtime will determine priority by id.

</article>

<article id="5">

## Service Rank
Let’s change our implementation and revalidate. Again: Think about your expectation. Make a mental or write it down.

```text/x-java
@Component(
    immediate = true,
    property = {
        "service.ranking:Integer=100"
    },
    service = HelloService.class
)
public class HelloUppercaseServiceImpl implements HelloService {
    @Override
    public String hello(String parameter) {
        return parameter.toUpperCase();
    }
}
```

**Note:** Developer Studio can redeploy your service automatically.

</article>

<article id="6">

## Check Gogo Shell

```text
g! lb helloworld
START LEVEL 20
   ID|State      |Level|Name
  590|Active     |    1|helloworld-api (1.0.0)
  591|Active     |    1|helloworld-service (1.0.0)
  592|Active     |    1|helloworld-command (1.0.0)
  593|Active     |    1|helloworld-capitalization-service (1.0.0)
g! stop 592
g! start 592
g! say hello
HELLO
```
So, there we are: The higher ranking service has been bound when the command has been restarted - it was the highest service rank available when they were looked up.

**Learn By Experimenting**

Figure out what happens when you stop the highest ranking service, and start it again. Think about your expectations beforehand.

</article>

<article id="7">

## Service References

**Static Binding**

You’ve learned that the reference to services defaults to being static and will only be reconsidered upon restarting the dependent service - in our case the Gogo shell command.

**Dynamic Binding**

But what if you want to be more dynamic? This works as well, but you’ll need to let the runtime know that you’d like to get an updated service whenever a better one is available. Here’s an implementation for such a case.

```text/x-java
@Component(
immediate = true,
    property = {
        "osgi.command.function=say",
        "osgi.command.scope=custom"
    },
    service = Object.class
)
public class HelloWorldCommand {
    
    public void say(String what) {
        System.out.println(helloService.hello(what));
    }
    
    @Reference(
               policy=ReferencePolicy.DYNAMIC,
               policyOption=ReferencePolicyOption.GREEDY,
               cardinality=ReferenceCardinality.MANDATORY)
    protected void setHelloService(HelloService helloService) {
        System.out.println("Setting " + helloService.getClass().getName());
        this.helloService = helloService;
    }
    
    protected void unsetHelloService(HelloService helloService) {
        System.out.println("Unsetting helloService " + helloService.getClass().getName());
        if (helloService == this.helloService) {
            this.helloService = null;
        }
    }

    private HelloService helloService;
}
```

**Note:** We now need a specific set and unset method for our reference, according to the OSGi specification:

###### Try It!

Deploy the new command, then restart the service implementations as you like. Validate the behavior of the system. Did the results match your expectations?

Well, at least you’ve seen one area where common expectation and the actual behavior of the runtime can differ. But that’s easy to fix, once you know what to expect.

</article>

<article id="8">

## More References

As promised: Liferay's documentation has fabulous chapters about more options. Because we explicitly only want to cover the bare minimum - please see the documentation for more details.

We hope you've gotten some good hints here and have a better understanding of the runtime behavior set:

+ [Overriding service references](https://customer.liferay.com/documentation/7.0/develop/tutorials/-/official_documentation/tutorials/overriding-service-references)
+ [Using Felix Gogo Shell](https://customer.liferay.com/documentation/7.0/develop/reference/-/official_documentation/reference/using-the-felix-gogo-shell)

</article>
