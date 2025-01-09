Part 1:

Now that we have seen a simple example of how authentication is working with a net core application,

with the Azure Active Directory, what we want to do now is let's say that main application is accessing

some API.

So in solution, let me add here a new project.

We will search for here and we will select the ASP.Net Core Web API.

Let's hit the next button.

And I will call this Azure ad API.

We won't have any authentication type.

We'll keep things simple and let's create this.

Now inside the solution Explorer right here.

Let me set the startup project as API for now and let me run that API.

Perfect.

You can see it is up and running.

And then right here I will just select IIS Express.

Now what we want to do is we will be using the Azure AD for authenticating this API as well.

So we will have to work on some NuGet packages inside the API itself.

Let's do manage NuGet packages.

And this time, since it's an API, we will be just needing the JWT bearer token.

I have the include pre-release because I'm using dotnet six.

We will install the latest version.

And that is the only package that we will require.

Next.

What I will do is insert the controller.

We have the weather forecast controller right here.

Let me just add the authorize attribute so that this won't be accessible by an unauthorized user.

You can do that at the global level or directly at the get method.

We'll do that globally right here.

With that in place.

If you run the API this time it won't work.

If you try to get and if you do try it out, it returns 500 and documented because no authentication

was specified.

So we have our API.

We need to make sure that this API is accessed by our main application using the authentication from

Azure Active Directory.

Part 2:

Now that we have added the API project in our solution, let's work on the portal for Azure.

So we'll go back to the main Azure Active Directory.

And here we just have one application.

We will have to create a new application for our API.

So we will say new registration and we will give it a name of Dotnet Mastery 80 and we will add API

at the end here.

Next is the supported account types.

We will keep them the same.

Now the reason we do not need a redirect Uri in this case is because the Azure ad will never have to

redirect to the API.

It will always redirect to the main client application.

That's why we will have an empty redirect Uri right here.

Once the API is created.

If we examine the API permission, this will be the same that we have seen with our client application.

So we are good with the default that we have here, but this API needs to be exposed because our main

client application will be consuming this API.

So for that you will have to select expose an API right here.

The first thing you see is the application ID Uri.

This is not set, so we will have to set that.

This will be the ID that will be in the claims, in the access token that grants access to our API.

We will select set here and then we'll just use the default right now.

Then the next thing for our API that we set here, we will have to add a scope if we examine our API

permission.

You can see here the Microsoft graph is the API name.

And then on that API we have a permission or scope of user dot read.

So in the API that we set right here, we want to add a scope.

Can call this as admin access.

Now you can be more explicit with that.

Like you can have read, write other access type that you want because in the API level you will be

able to read the scope and you can grant access based on the scope that they have.

Next we have who can consent.

We want both admin as well as regular users to consent.

We will enter the values for display, name and description.

And let's add the scope.

The values that we added will end up on the consent screen that will be displayed on the page.

So with this, all the configuration in Azure is done.

We just need to add bearer tokens in our API and see things in action.

Part 3:

Now, if we switch back to the application, we have to configure the startup class file of our API

project.

If we scroll down to the configure services like we did inside the Client project, which is the web

project here, we will also need authentication.

So inside configure services, we will say services dot add authentication.

The scheme that we want to pass here is better.

So rather than using magic string, we will use JWT bearer defaults using the namespace and authentication

scheme that has the constant value of bearer right here.

And then on this we will actually add the JWT bearer here.

We will have to give the scheme name so we can copy this and paste it one more time.

Next we will have to configure the options.

Here, we will have to configure two options.

First is the authority I will keep empty right now and the next one will be the audience.

The authority will be saying that we have inside our client application.

So if we go there, we have the authority, we can copy this and we'll paste it right here because that

will be the main authority that will be generating all the tokens.

Next is the audience.

Audience is the API ID that we generated in the last video.

So if we go back, we have the API ID right here.

We will copy this and we will paste that.

Last thing that is needed is to add the authentication middleware to our pipeline.

So we'll have app dot add authentication before we add the authorization.

So with that, our API configuration looks great.

The next thing that we have to do is from our main client application, we have to call the weather

forecast controller and the API endpoint inside that.