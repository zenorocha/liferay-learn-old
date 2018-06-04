---
title: "Personal Pages"
description: "Personal Pages"
layout: "guide"
icon: "cloud"
weight: 4
---

###### {$page.description}

<article id="1">

## Creating personal pages for all your users

When you look at the default settings in portal.properties, you’ll see that one page for the dashboard (“private”) and profile (“public”) is automatically generated for each user. And if you wondered how to change their look&feel, or content, or how to add or remove functionality: This is your chapter.

### First: Let's deactivate the default

Let’s set up our system so that it is all managed the same way: We’ll deactivate the crude default dashboards and profiles in portal.properties, and then maintain all user’s personal pages through User Groups and Site Templates.

If you have portal-ext.properties in your installation’s home directory already, open it. Otherwise create it. When you can’t remember what the home directory is, or what portal-ext.properties contains, check back with the Basic Configuration chapter.

Add the following lines to the file (bold lines are changed values from default, italics are unchanged default values for your information)

    #
    # Set whether or not private layouts are enabled. Set whether or not private
    # layouts should be auto created if a user has no private layouts. If
    # private layouts are not enabled, then the property
    # "layout.user.private.layouts.auto.create" is assumed to be false.
    #
    # layout.user.private.layouts.enabled=true
    layout.user.private.layouts.auto.create=false

    #
    # Set whether or not public layouts are enabled. Set whether or not public
    # layouts should be auto created if a user has no public layouts. If public
    # layouts are not enabled, then the property
    # "layout.user.public.layouts.auto.create" is assumed to be false.
    #
    # layout.user.public.layouts.enabled=true
    layout.user.public.layouts.auto.create=false


<aside>

### <span class="icon-16-star"></span> Pro Tip

Unfortunately, changes to portal-ext.properties require a restart of Liferay in order to be picked up.

When you're on Windows, and File-Explorer doesn't show file extensions by default, make sure that the actual file name is portal-ext.properties, e.g. the file type should be shown as "properties", not as "text file" or "txt file"

</aside>

</article>
<article id="2">

After this configuration step and the associated restart, you'll easily see that the default pages are no longer there. Now we'll need to create new pages, that we can maintain through the user interface, and assign to the people that we want to have them associated with.

### Create a Site Template, consisting of one page with two portlets

Navigate to "Control Panel / Sites / Site Templates". Add a new Site Template named "User Dashboard". Then click on the Template's name to navigate to the default page of this template. You'll see an empty page, which you can change to your heart's content: Make it a 2-column layout, adding the "Profile" widget to the left column and the "Activities" widget to the right column. 

Both applications will show content from a user's profile, e.g. a user's name and profile image, or recent message board participation or blog articles.

You may create more pages if you like, but it's not necessary. Page creation works just like with regular pages. You'll see that some widgets behave differently than on regular pages though - explore what that means. There are certain widgets that are meant to be on personal pages, like the Profile and Activities portlet, that need the context from those pages and can't be used on regular pages.

### Create user group

Now that we want to assign this Site Template to more users, we need to do so via a User Group. Go to "Control Panel / Users / User Groups", and create a new User Group named "Dashboarders". In "My Dashboard", select the Site Template that you've just created.

### Assign user group to own user

When you've saved the User Group, select "Assign Members" from the User Group's action menu (or just click the User Group's name) and make your own user account a member of this user group.

Alternatively, you could also go to "Control Panel / Users / Users and Organizations", find your own user account and edit it - go to the "Memberships" section and add yourself to the "Dashboarders" user group. 

### check Dashboard / Profile

Click on your user name in the left column and validate that "My Dashboard" can be navigated to. Do so and validate that the portlets from the Site Template now exist in your dashboard, showing your personal information (at this time of the training, you probably won't have any "activities")

### assign user group to *all* users by default

Assigning such User Groups individually won't scale. We can apply this User Group to all users by default. For this, go to "Control Panel / Configuration / Instance Settings" and open the "Users" section, click "Default User Associations" and enter "Dashboarders" (or the name of your user group) under "User Groups". Observe the little help icon, where it says "Enter the default user group names per line that are associated with newly created users". You can optionally also check the checkbox "apply to existing users".

</article>
 
