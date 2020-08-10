# Readings: Claims and JWT tokens
## Claims-Based auth
* When an identity is created it may be assigned one or more claims issued by a trusted party. A claim is a name value pair that represents what the subject is, not what the subject can do. For example, you may have a driver's license, issued by a local driving license authority. Your driver's license has your date of birth on it. In this case the claim name would be DateOfBirth, the claim value would be your date of birth, for example 8th June 1970 and the issuer would be the driving license authority. Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value. For example if you want access to a night club the authorization process might be:
* The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.
* An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.
* Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource. Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.
* The simplest type of claim policy looks for the presence of a claim and doesn't check the value.
* `[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}`
* In the above example any identity which fulfills the EmployeeOnly policy can access the Payslip action as that policy is enforced on the controller. However in order to call the UpdateSalary action the identity must fulfill both the EmployeeOnly policy and the HumanResources policy.
## Introduction to Authentication with ASP.NET Core
* First of all, we should clarify the difference between these two dependent facets of security. The simple answer is that Authentication is the process of determining who you are, while Authorisation revolves around what you are allowed to do, i.e. permissions. Obviously before you can determine what a user is allowed to do, you need to know who they are, so when authorisation is required, you must also first authenticate the user in some way.
* The fundamental properties associated with identity have not really changed in ASP.NET Core - although they are different, they should be familiar to ASP.NET developers in general. For example, in ASP.NET 4.x, there is a property called User on HttpContext, which is of type IPrincipal, which represents the current user for a request. In ASP.NET Core there is a similar property named User, the difference being that this property is of type ClaimsPrincipal, which implements IPrincipal.
* The move to use ClaimsPrincipal highlights a fundamental shift in the way authentication works in ASP.NET Core compared to ASP.NET 4.x. Previously, authorisation was typically Role-based, so a user may belong to one or more roles, and different sections of your app may require a user to have a particular role in order to access it. In ASP.NET Core this kind of role-based authorisation can still be used, but that is primarily for backward compatibility reasons. The route they really want you to take is claims-based authentication.
* The concept of claims-based authentication can be a little confusing when you first come to it, but in practice it is probably very similar to approaches you are already using. You can think of claims as being a statement about, or a property of, a particular identity. That statement consists of a name and a value. For example you could have a DateOfBirth claim, FirstName claim, EmailAddress claim or IsVIP claim. Note that these statements are about what or who the identity is, not what they can do.
* The identity itself represents a single declaration that may have many claims associated with it. For example, consider a driving license. This is a single identity which contains a number of claims - FirstName, LastName, DateOfBirth, Address and which vehicles you are allowed to drive. Your passport would be a different identity with a different set of claims.
* So lets take a look at that in the context of ASP.NET Core. Identities in ASP.NET Core are a ClaimsIdentity. A simplified version of the class might look like this (the actual class is a lot bigger!):

`public class ClaimsIdentity: IIdentity
{
    public string AuthenticationType { get; }
    public bool IsAuthenticated { get; }
    public IEnumerable<Claim> Claims { get; }

    public Claim FindFirst(string type) { /*...*/ }
    public Claim HasClaim(string type, string value) { /*...*/ }
}`
* I have shown the main properties in this outline, including Claims which consists of all the claims associated with an identity. There are a number of utility methods for working with the Claims, two of which I have shown here. These are useful when you come to authorisation, and you are trying to determine whether a particular Identity has a given Claim you are interested in.
* The AuthenticationType property is fairly self-explanatory. In our practical example previously, this might be the string Passport or DriversLicense, but in ASP.NET it is more likely to be Cookies, Bearer, or Google etc. It's simply the method that was used to authenticate the user, and to determine the claims associated with an identity.
* Finally, the property IsAuthenticated indicates whether an identity is authenticated or not. This might seem redundant - how could you have an identity with claims when it is not authenticated? One scenario may be where you allow guest users on your site, e.g. on a shopping cart. You still have an identity associated with the user, and that identity may still have claims associated with it, but they will not be authenticated. This is an important distinction to bear in mind.
* As an adjunct to that, in ASP.NET Core if you create a ClaimsIdentity and provide an AuthenticationType in the constructor, IsAuthenticated will always be true. So an authenticated user must always have an AuthenticationType, and, conversely, you cannot have an unauthenticated user which has an AuthenticationType.
## Part 2: JWT to authenticate Servers API’s
* JSON Web Token (JWT) is a means of representing claims to be transferred between two parties. The claims in a JWT are encoded as a JSON object that is digitally signed using JSON Web Signature (JWS) and/or encrypted using JSON Web Encryption (JWE).
* In simple terms, it is just another way of encoding JSON object and use that encoded object as access tokens for authentication from the server.
* This is the second part of the series of two shorts post regarding the practical application of JWT.
JWT for downloading the files at the client.
JWT for the server to server authentication (current blog post).
* alg: We have two main algorithms(HS256/RS256) to sign our JWT 3rd part (Signature) which we mention in the headers so that the producer and consumer(you will understand this soon in the next section) both should use the same algorithm to verify the token on each end. HS256 indicates that this token is signed using HMAC-SHA256.
* When we base64UrlEncode the above header data we will get eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9 the first part of our JWT token.
* It mostly contains the claims (custom data) and some standard claims as well. We can use standard claims to identify a lot of things i.e exp: the expiry of the token,iat: time at which token issued etc.
* Audience (aud) - The "aud" (audience) claim identifies the recipients that the JWT is intended for. Each principal intended to process the JWT must identify itself with a value in the audience claim. If the principal processing the claim does not identify itself with a value in the aud claim when this claim is present, then the JWT must be rejected.
* Expiration time (exp) - The "exp" (expiration time) claim identifies the expiration time on or after which the JWT must not be accepted for processing. The value should be in NumericDate[10][11] format.
* It is calculated by base64url encoding of header and payload and concatenating them with a period as a separator. Then encrypt it with HMAC-SHA256 along with the secret key and the result of the first step.
* There are two parties involved, one party who gives a service, and the other party who uses the service.
Producer is the one who gives a service. It will be the provider(Server) of the API(s) which are JWT protected.
Consumer is the one who uses it. It will be the customer(Server/Mobile App/ Web App/ Client) who will be providing the valid JWT token to consume the API(s) being provided by the Producer.
* 1. Share the SECRET: This is the responsibility of the Producer side to share the mutual secret. This secret will be required to verify the token at the Producer end and the same secret will be used to create the token at the respective consumer side.
* 2. Prepare the PAYLOAD: Consumer should encode all the data (body or query or params) in the payload of the JWT token (you can choose specific fields that need to be present in the payload of JWT but I would suggest to wrap all the data). We will exact this at producer end to verify that the data is the same in the token payload and the request API.
i.e: GET call to `/v1/api/getdetails?email=rachitgulati26@gmail.com` should have JWT payload
