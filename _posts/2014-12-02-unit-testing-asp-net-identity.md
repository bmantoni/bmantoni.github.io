---
layout: post
category: blog
published: true
title: "Unit Testing ASP.NET Identity "
---

## Mocking ASP.NET Identity for Web API and MVC

In the earlier days, it was quite a pain to test any built-in .NET classes, especially before the days of MVC. Even after MVC, using anything like the HTTPContext or Membership APIs directly from your Controller logic could end up making things difficult or impossible to test, since you'd be hitting concrete classes with no interfaces, static classes, or other things difficult-to-mock/stub.

Typically I'd end up with layers of wrappers to decouple my code and make it testable and modular. But how far things have come. 

An example - I have a Web API ApiController that I'm using to authenticate the same username/password credentials created through MVC. This lets me share the same user management pages, persistence, password policies, etc... in both the API and web interface. When testing, I don't want to re-test Microsoft's code - I only want to test how my code behaves. It turns out I haven't needed to introduce any wrappers - things are now very nearly designed to allow you to swap things in where needed.

First, I need a way to access the UserManager from my ApiController. Here we see another nice addition to the .NET stack: Owin. Again, it's a nice layer between the .NET runtime/app server/modules and my own code, and can provide something like a Service Locator. I just add this property to my ApiController:
{% highlight c# %}
private ApplicationUserManager _userManager;
public ApplicationUserManager UserManager
{
    get
    {
        if (_userManager == null)
        {
            _userManager = Request.GetOwinContext().GetUserManager<ApplicationUserManager>();
        }
        return _userManager;
    }
    set //public for tests only
    {
        _userManager = value;
    }
}
{% endhighlight %}

Which relies on an extension method I add somewhere:

{% highlight c# %}
public static IOwinContext GetOwinContext(this HttpRequestMessage request)
{
    var context = request.Properties["MS_HttpContext"] as HttpContextWrapper;
    if (context != null)
    {
        return HttpContextBaseExtensions.GetOwinContext(context.Request);
    }
    return null;
}
{% endhighlight %}

Now, to setup my tests I use this method - calling it on my controller after instantiating it. It injects a mocked ApplicationUserManager into the controller before I act on it. It prepares a valid user to be authenticated against (or not) by mocking the underlying IUserStore. Moq makes this quite elegant by offering the .As operation - which dovetails nicely with the UserManager's discovery of interface implementations.

{% highlight c# %}
private void MockUserManager(MyController c)
{
    var hpw = new PasswordHasher().HashPassword("testpass");
    var validTestUser = new ApplicationUser() { Id = "1", UserName = "testuser", PasswordHash = hpw };

    var us = new Mock<IUserStore<ApplicationUser>>(MockBehavior.Strict);
    us.Setup(p => p.FindByNameAsync("testuser")).ReturnsAsync(validTestUser);
    us.As<IUserPasswordStore<ApplicationUser>>()
        .Setup(p => p.GetPasswordHashAsync(It.IsAny<ApplicationUser>())).ReturnsAsync(hpw);
    var aum = new ApplicationUserManager(us.Object);

    c.UserManager = aum;
}
{% endhighlight %}

Now, with everything arranged code my my controller like `var user = UserManager.Find(username, password);` will operate on the data loaded in the test setup. Sweet!