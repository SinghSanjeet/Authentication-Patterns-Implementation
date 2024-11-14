required packages:
 Microsoft.AspNetCore.Authentication.OpenIDConnect
 Microsoft.Identity.web

Implicit grant and hybrid flows
Request a token directly from the authorization endpoint. If the application has a
single-page architecture (SPA) and doesn't use the authorization code flow, or if it
invokes a web API via JavaScript, select both access tokens and ID tokens. For ASP.NET Core
web apps and other web apps that use hybrid authentication, 
select only ID tokens. Learn more about tokens.