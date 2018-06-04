---
title: "User Management"
description: "User Management"
layout: "guide"
icon: "cloud"
weight: 4
---

###### {$page.description}

<article id="1">

## User Management


We didn’t do a lot in Liferay yet, but in order to change something, we’ll need to sign in - and that’s where User Management comes in.
A platform like Liferay has you covered with User Management - and what you’ve seen is only the basics. We can do a lot more, but will only show this after introducing content-related topics. Why stick with user management for too long in the beginning, when there’s still no content? Well, we don’t.

Here’s your takeaway from this chapter, together with an outlook on the later chapters - to help you paint a mental picture.


<aside>

### <span class="icon-16-star"></span> Pro Tip

Check your own installation for all the features that you've seen in the previous video. You should be able to find all of the different places, even though the layout might differ slightly because the video has been recorded on a previous version of Liferay (DXP 7.0).

</aside>

</article>
<article id="2">

### Your Takeaways


#### User Management

- Users always have an entry in Liferay’s Database
- It’s possible to interface with external user management system (not yet handled), but users will always be in Liferay’s Database
- User accounts may be active (e.g. can log in) or inactive (can’t log in)
- Users have almost no permissions by default, other than maintaining their own profile
- Later we’ll even introduce mechanics to follow individual users around when they’re not signed in.

#### Grouping Users

In a sufficiently large installation (let’s say, more than 10 registered users) you probably do not want to grant permissions on a user-by-user-basis. This means that we’ll need a way to talk about multiple users at once. Liferay provides two mechanisms:

##### Organization:

- can be arranged in a hierarchy. The hierarchy can have multiple root organizations - e.g. is not limited to a single root
- Membership in a child-organization (in a hierarchy) automatically implies membership in the parent organization
- can “have” a site (what’s a site you ask? One of the very next chapters - if not the next - will cover this)
- typically used for mirroring some underlying structure (potentially hierarchical) in the real world - e.g. a company and its departments or locations.

##### User Groups: 

- No Hierarchy
- have as many as you like to handle groups of users, wherever you’d like to handle individual users
- Can have site templates to build personal sites (a feature that will be handled in a later chapter)
- are arbitrary collections that can be done for whatever reason - they might not even have equivalent natural groups in the physical world. 
- often User Groups are used to import LDAP-Groups (if you interface with LDAP, which we’ll cover in a later chapter)

What you might find when you explore these options…

You’ll see that you can also create a “Location” as an Organization. This is a special type of Organizations that can’t have any more siblings - e.g. it’s the lower end of the hierarchy. You may use it if you definitely want to document that there can’t be any more siblings. Note that you can’t change the organization type later on - you’ll stay more flexible when you just use the standard type (Organization) and simply not add any child sites to it)

</article>
<article id="3">

### Exercise
Think of your usecase for Liferay and make a sketch for the structure of your user base. 
If you have only a manageable size of (known) users, you might choose to create accounts that you’ll use throughout this course for the various responsibilities that these users have. 
If you have a huge amount of users, you might want to choose 3-4 example personas and create accounts for them. For the purpose of this course, you'll be the one impersonating your users, so make use of their names, email addresses and passwords.

<aside>

If you have an installation that actually sends out mails, and you're looking for actual mail addresses but don't want to create many, you may try out this trick that some providers (e.g. gmail) support: If your email address is example@gmail.com, then example+admin@gmail.com, example+regularuser@gmail.com etc will all be delivered to your main address - you can make up anything between the "+" and "@" sign.

</aside>

Then think of the content that you’ll have in your portal. 

- Will it be different content, e.g. department by department? Or for individual projects?
- Will the users be signing up for areas of their interest by themselves or will their access be centrally managed?
- Will you have largely public content, visible to everybody anyways?
- Or will it be largely private content, that requires logging in 
- How many people will have write access - and will they have write access to individual areas, or to all of the content on your site?

Sketch them out and have them ready when we build content for the site.

</article>
