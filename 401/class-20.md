# Readings: Data Transfer Objects
## Data Transfer Objects
* Right now, our web API exposes the database entities to the client. The client receives data that maps directly to your database tables. However, that's not always a good idea. Sometimes you want to change the shape of the data that you send to client. For example, you might want to:

Remove circular references (see previous section).
Hide particular properties that clients are not supposed to view.
Omit some properties in order to reduce payload size.
Flatten object graphs that contain nested objects, to make them more convenient for clients.
Avoid "over-posting" vulnerabilities. (See Model Validation for a discussion of over-posting.)
Decouple your service layer from your database layer.
* To accomplish this, you can define a data transfer object (DTO). A DTO is an object that defines how the data will be sent over the network. Let's see how that works with the Book entity. In the Models folder, add two DTO classes:
* `namespace BookService.Models
{
    public class BookDto
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string AuthorName { get; set; }
    }
}

namespace BookService.Models
{
    public class BookDetailDto
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public int Year { get; set; }
        public decimal Price { get; set; }
        public string AuthorName { get; set; }
        public string Genre { get; set; }
    }
}`
## How to use DTOs
* A Data Transfer Object (commonly known as a DTO) is usually an instance of a POCO (plain old CLR object) class used as a container to encapsulate data and pass it from one layer of the application to another. You would typically find DTOs being used in the service layer to return data back to the presentation layer. The biggest advantage of using DTOs is decoupling clients from your internal data structures.
* First off, let’s create an ASP.NET Core project in Visual Studio. Assuming Visual Studio 2019 is installed in your system, follow the steps outlined below to create a new ASP.NET Core API project in Visual Studio.

Launch the Visual Studio IDE.
Click on “Create new project.”
In the “Create new project” window, select “ASP.NET Core Web Application” from the list of the templates displayed.
Click Next. 
In the “Configure your new project” window, specify the name and location for the new project.
Click Create. 
In the “Create New ASP.NET Core Web Application” window shown next, select .NET Core as the runtime and ASP.NET Core 3.1 (or later) from the drop-down list at the top.
Select “API” as the project template to create a new ASP.NET Core API application. 
Ensure that the check boxes “Enable Docker Support” and “Configure for HTTPS” are unchecked as we won’t be using those features here.
Ensure that Authentication is set as “No Authentication” as we won’t be using authentication either.
Click Create. 
This will create a new ASP.NET Core API project in Visual Studio. We’ll use this project to work with Data Transfer Objects in the subsequent sections of this article.
* When designing and developing an application, if you’re using models to pass data between the layers and sending data back to the presentation layer, then you’re exposing the internal data structures of your application. That’s a major design flaw in your application.
* By decoupling your layers DTOs make life easier when you’re implementing APIs, MVC applications, and also messaging patterns such as Message Broker. A DTO is a great choice when you would like to pass a lightweight object across the wire — especially when you’re passing your object via a medium that is bandwidth-constrained.
* You can take advantage of DTOs to abstract the domain objects of your application from the user interface or the presentation layer. In doing so, the presentation layer of your application is decoupled from the service layer. So if you would like to change the presentation layer, you can do that easily while the application will continue to work with the existing domain layer. Similarly, you can change the domain layer of your application without having to change the presentation layer of the application.
* Another reason you would want to use DTOs is data hiding. That is, by using DTOs you can return only the data requested. As an example, assume you have a method named GetAllEmployees() that returns all the data pertaining to all employees. Let’s illustrate this by writing some code.
* In the project we created earlier, create a new file called Employee.cs. Write the following code inside this file to define a model class named Employee.
* `public class Employee
    {
        public int Id { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string DepartmentName { get; set; }
        public decimal Basic { get; set; }
        public decimal DA { get; set; }
        public decimal HRA { get; set; }
        public decimal NetSalary { get; set; }
    }`
* Note the Employee class contains properties including Id, FirstName, LastName, Department, Basic, DA, HRA, and NetSalary. However, the presentation layer might only need the Id, FirstName, LastName, and Department Name of the employees from the GetAllEmployees() method. If this method returns a List<Employee> then anyone would be able to see the salary details of an employee. You don’t want that. 