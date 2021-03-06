# Readings: Intro to ERDs
* In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules. Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.
* For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date. By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data. To see an example of how to do that, you'll add an attribute to the EnrollmentDate property in the Student class.
* The DataType attribute is used to specify a data type that's more specific than the database intrinsic type. In this case we only want to keep track of the date, not the date and time. The DataType Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more. The DataType attribute can also enable the application to automatically provide type-specific features. For example, a mailto: link can be created for DataType.EmailAddress, and a date selector can be provided for DataType.Date in browsers that support HTML5. The DataType attribute emits HTML 5 data- (pronounced data dash) attributes that HTML 5 browsers can understand. The DataType attributes don't provide any validation.
* DataType.Date doesn't specify the format of the date that's displayed. By default, the data field is displayed according to the default formats based on the server's CultureInfo.
* You can use the DisplayFormat attribute by itself, but it's generally a good idea to use the DataType attribute also. The DataType attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:
* * The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.)
* * By default, the browser will render data using the correct format based on your locale.
* The StringLength attribute won't prevent a user from entering white space for a name. You can use the RegularExpression attribute to apply restrictions to the input. For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:
* You can also use attributes to control how your classes and properties are mapped to the database. Suppose you had used the name FirstMidName for the first-name field because the field might also contain a middle name. But you want the database column to be named FirstName, because users who will be writing ad-hoc queries against the database are accustomed to that name. To make this mapping, you can use the Column attribute.
* The Column attribute specifies that when the database is created, the column of the Student table that maps to the FirstMidName property will be named FirstName. In other words, when your code refers to Student.FirstMidName, the data will come from or be updated in the FirstName column of the Student table. If you don't specify column names, they're given the same name as the property name.
* The Required attribute makes the name properties required fields. The Required attribute isn't needed for non-nullable types such as value types (DateTime, int, double, float, etc.). Types that can't be null are automatically treated as required fields.
* The Required attribute must be used with MinimumLength for the MinimumLength to be enforced.
* The Display attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).
* FullName is a calculated property that returns a value that's created by concatenating two other properties. Therefore it has only a get accessor, and no FullName column will be generated in the database.
* There's a one-to-zero-or-one relationship between the Instructor and the OfficeAssignment entities. An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity. But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention. Therefore, the Key attribute is used to identify it as the key:
* You can also use the Key attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.
* The Instructor entity has a nullable OfficeAssignment navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable Instructor navigation property (because an office assignment can't exist without an instructor -- InstructorID is non-nullable). When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.
* The course entity has a foreign key property DepartmentID which points to the related Department entity and it has a Department navigation property.
* The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity. EF automatically creates foreign keys in the database wherever they're needed and creates shadow properties for them. But having the foreign key in the data model can make updates simpler and more efficient. For example, when you fetch a course entity to edit, the Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity. When the foreign key property DepartmentID is included in the data model, you don't need to fetch the Department entity before you update.
* By default, Entity Framework assumes that primary key values are generated by the database. That's what you want in most scenarios. However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.
* The foreign key properties and navigation properties in the Course entity reflect the following relationships:
* A course is assigned to one department, so there's a DepartmentID foreign key and a Department navigation property for the reasons mentioned above.
* Earlier you used the Column attribute to change column name mapping. In the code for the Department entity, the Column attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:
* Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property. The CLR decimal type maps to a SQL Server decimal type. But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.
* A department may or may not have an administrator, and an administrator is always an instructor. Therefore the InstructorID property is included as the foreign key to the Instructor entity, and a question mark is added after the int type designation to mark the property as nullable. The navigation property is named Administrator but holds an Instructor entity:
* 