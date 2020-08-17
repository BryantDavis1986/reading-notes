# Read: Class 31 - Sending Emails
## Readings: SendGrid
* SendGrid is a cloud-based email service that provides reliable transactional email delivery, scalability, and real-time analytics along with flexible APIs that make custom integration easy. Common SendGrid use cases include:

Automatically sending receipts or purchase confirmations to customers.
Administering distribution lists for sending customers monthly fliers and promotions.
Collecting real-time metrics for things like blocked email and customer engagement.
Forwarding customer inquiries.
Processing incoming emails.
* Use the SendGridMessage object to create an email message. Once the message object is created, you can set properties and methods, including the email sender, the email recipient, and the subject and body of the email.
* After creating an email message, you can send it using SendGrid's API. Alternatively, you may use .NET's built in library.

Sending email requires that you supply your SendGrid API Key. If you need details about how to configure API Keys, please visit SendGrid's API Keys documentation.

You may store these credentials via your Azure portal by clicking Application settings and adding the key/value pairs under App settings.
* The below example can be used to send a single email to multiple persons from the ASP .NET Core API using the MailHelper class of SendGrid.Helpers.Mail namespace. For this example we are using ASP .NET Core 1.0.

In this example, the API key has been stored in the appsettings.json file which can be overridden from the Azure portal as shown in the above examples.
* First, we need to add the below code in the Startup.cs file of the .NET Core API project. This is required so that we can access the SENDGRID_API_KEY from the appsettings.json file by using dependency injection in the API controller. The IConfiguration interface can be injected at the constructor of the controller after adding it in the ConfigureServices method below. The content of Startup.cs file looks like the following after adding the required code:
* At the controller, after injecting the IConfiguration interface, we can use the CreateSingleEmailToMultipleRecipients method of the MailHelper class to send a single email to multiple recipients. The method accepts one additional boolean parameter named showAllRecipients. This parameter can be used to control whether email recipients will be able to see each others email address in the To section of email header. The sample code for controller should be like below
* Attachments can be added to a message by calling the AddAttachment method and minimally specifying the file name and Base64 encoded content you want to attach. You can include multiple attachments by calling this method once for each file you wish to attach or by using the AddAttachments method. The following example demonstrates adding an attachment to a message:
* SendGrid provides additional email functionality through the use of mail settings and tracking settings. These settings can be added to an email message to enable specific functionality such as click tracking, Google analytics, subscription tracking, and so on. For a full list of apps, see the Settings Documentation.

Apps can be applied to SendGrid email messages using methods implemented as part of the SendGridMessage class. The following examples demonstrate the footer and click tracking filters:

The following examples demonstrate the footer and click tracking filters: