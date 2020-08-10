# Readings: Intro to Identity
* ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps. Users can create an account with the login information stored in Identity or they can use an external login provider. Supported external login providers include Facebook, Google, Microsoft Account, and Twitter.
* Identity can be configured using a SQL Server database to store user names, passwords, and profile data. Alternatively, another persistent store can be used, for example, Azure Table Storage.
* AddDefaultIdentity was introduced in ASP.NET Core 2.1. Calling AddDefaultIdentity is similar to calling the following:

AddIdentity
AddDefaultUI
AddDefaultTokenProviders
* Select File > New > Project.
Select ASP.NET Core Web Application. Name the project WebApp1 to have the same namespace as the project download. Click OK.
Select an ASP.NET Core Web Application, then select Change Authentication.
Select Individual User Accounts and click OK.
* The generated project provides ASP.NET Core Identity as a Razor Class Library. The Identity Razor Class Library exposes endpoints with the Identity area. For example:

/Identity/Account/Login
/Identity/Account/Logout
/Identity/Account/Manage
* Run the app and register a user. Depending on your screen size, you might need to select the navigation toggle button to see the Register and Login links.
* The Login form is displayed when:

The Log in link is selected.
A user attempts to access a restricted page that they aren't authorized to access or when they haven't been authenticated by the system.
* To explore Identity in more detail:

Create full identity UI source
Examine the source of each page and step through the debugger.
* All the Identity dependent NuGet packages are included in the Microsoft.AspNetCore.App metapackage.

The primary package for Identity is Microsoft.AspNetCore.Identity. This package contains the core set of interfaces for ASP.NET Core Identity, and is included by Microsoft.AspNetCore.Identity.EntityFrameworkCore.
## ASP.NET Core 2.0 Authentication and Authorization System Demystified
* There is a component that exists in ASP.NET Core that conjures up an enchanted shield that protects portions (or all) of your website from unauthorized access. Like many people, I have used this component from the beginning of my journey, but have never understood it. It was conjured up by a wizard and provided a magical barrier between my website and the world. That’s not how it really works, of course, but without the right knowledge, it might as well.
* While trying to figure out how to fix an error in my code, I happened to ask the right question at the right time on the right Slack channel. David Fowler, who happens to be one of the core maintainers of aspnetcore, decided to give everyone present a lesson on how the authentication system (auth system, from now on) works in ASP.NET Core 2.0. This article is based on the information in his impromptu lesson.
* I later found an article by Andrew Lock going into details about the claims portion and, after reading his article and doing slightly more research, added the section on Identity.
* Understanding the system first requires understanding its components and behaviors. They can be broken down into identity, verbs, authentication handlers, and middleware. I will cover each of these individually and then demonstrate how they work together in an example auth request. Since ASP.NET Core's most common authentication handler is the Cookies auth handler, these examples will use cookie authentication.
* Key to understanding how authentication works is to first understand what an identity is in ASP.NET Core 2.0. There are three classes which represent the identity of a user: Claim, ClaimsIdentity, and ClaimsPrincipal
* A Claim represents a single fact about the user. It could be the user's first name, last name, age, employer, birth date, or anything else that is true about the user. A single claim will contain only a single piece of information. A claim representing something about a user John Smith could be his first name: John. A second claim would be his last name: Smith.
* Claims are represented by the Claim class in ASP.Net Core. It's most common constructor accepts two strings: type and value. The 'type' parameter is the name of the claim, while the value is the information the claim is representing about the user.
* An Identity represents a form of identification or, in other words, a single way of proving who you are. In real life this could be a driver's license. In ASP.Net Core, it is a ClaimsIdentity. This class represents a single form of digital identification.
* A single instance of a ClaimsIdentity can be authenticated or not authenticated. According to Andrew Lock's Introduction to Authentication with ASP.NET Core, simply setting the AuthenticationType will automatically ensure the IsAuthenticated property is true. This is because if you have authenticated the identity in any way, then it must, by definition, be authenticated.
* An unknown person walking up to you and making various claims about theirself and their life would be equivilent to an unauthenticated ClaimsIdentity. Lock writes that this may be useful to allow guest shopping carts (prior to a sign in, perhaps) and similar use cases.
* A driver's license contains many claims about its subject: first and last names, birthdate, hair and eye colors, height, and others. Similarly, a ClaimsIdentity can contain numerous claims about a user.
* A Principal represents the actual user. It can contain one or more instances of ClaimsIdentity, just like in life a person may have a driver's license, concealed-carry permit, social security card, and a passport. Each of the identities is used for a different purpose and may contain a unique set of claims, but they all identify the same user in some form or another.
* To sum up, a ClaimsPrincipal represents a user and contains one or more instances of ClaimsIdentity, which in turn represent a single form of identification and contain one or more instances of Claim, which represents a single piece of information about a user. The ClaimsPrincipal is what the HttpContext.SignInAsync method accepts and passes to the specified AuthenticationHandler.
* There are 5 verbs (these can also be thought of as commands or behaviors) that are invoked by the auth system, and are not necessarily called in order. These are all independent actions that do not communicate among themselves, however, when used together allow users to sign in and access pages otherwise denied. Here is a very brief description of what each verb is responsible for. We’ll go into more depth further in the article.
* Authenticate
Gets the user’s information if any exists (e.g. decoding the user’s cookie, if one exists)
Challenge
Requests authentication by the user (e.g. showing a login page)
SignIn
Persists the user’s information somewhere (e.g. writes a cookies)
SignOut
Removes the user’s persisted information (e.g. deletes the cookies)
Forbid
Denies access to a resource for unauthenticated users or authenticated but unauthorized users (e.g. displaying a “not authorized” page)
* Authentication handlers are components that actually implement the behavior of the 5 verbs above. The default auth handler provided by ASP.NET Core is the Cookies authentication handler which implements all 5 of the verbs. It is important to note, however, that an auth handler is not required to implement all of the verbs. Oauth handlers, for instance, do not implement the SignIn verb, but rather pass off that responsibility to another auth handler, such as the Cookies auth handler.
* Authentication handlers must be registered with the auth system in order to be used and are associated with schemes. A scheme is just a string that identifies a unique auth handler in a dictionary of auth handlers. The default scheme for the Cookies auth handler is “Cookies”, but it can be changed to anything. Multiple auth handlers can be used side by side, and sometimes (like in the earlier example of the Oauth handlers) use functionality provided by other auth handlers.
* 