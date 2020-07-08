# Readings: System.IO
## File and Stream I/O
* File and stream I/O (input/output) refers to the transfer of data either to or from a storage medium. In the .NET Framework, the System.IO namespaces contain types that enable reading and writing, both synchronously and asynchronously, on data streams and files. These namespaces also contain types that perform compression and decompression on files, and types that enable communication through pipes and serial ports.
* A file is an ordered and named collection of bytes that has persistent storage. When you work with files, you work with directory paths, disk storage, and file and directory names. In contrast, a stream is a sequence of bytes that you can use to read from and write to a backing store, which can be one of several storage mediums (for example, disks or memory). Just as there are several backing stores other than disks, there are several kinds of streams other than file streams, such as network, memory, and pipe streams.
### Files and directories
* File - provides static methods for creating, copying, deleting, moving, and opening files, and helps create a FileStream object.
* FileInfo - provides instance methods for creating, copying, deleting, moving, and opening files, and helps create a FileStream object.
* Directory - provides static methods for creating, moving, and enumerating through directories and subdirectories.
* DirectoryInfo - provides instance methods for creating, moving, and enumerating through directories and subdirectories.
* Path - provides methods and properties for processing directory strings in a cross-platform manner.
* You should always provide robust exception handling when calling filesystem methods. For more information, see Handling I/O errors.
* In addition to using these classes, Visual Basic users can use the methods and properties provided by the Microsoft.VisualBasic.FileIO.FileSystem class for file I/O.
### Streams
* Reading - transferring data from a stream into a data structure, such as an array of bytes.
* Writing - transferring data to a stream from a data source.
* Seeking - querying and modifying the current position within a stream.
* FileStream – for reading and writing to a file.
* IsolatedStorageFileStream – for reading and writing to a file in isolated storage.
* MemoryStream – for reading and writing to memory as the backing store.
* BufferedStream – for improving performance of read and write operations.
* NetworkStream – for reading and writing over network sockets.
* PipeStream – for reading and writing over anonymous and named pipes.
* CryptoStream – for linking data streams to cryptographic transformations.
### Readers and writers
* The System.IO namespace also provides types for reading encoded characters from streams and writing them to streams. Typically, streams are designed for byte input and output. The reader and writer types handle the conversion of the encoded characters to and from bytes so the stream can complete the operation. Each reader and writer class is associated with a stream, which can be retrieved through the class's BaseStream property.
* BinaryReader and BinaryWriter – for reading and writing primitive data types as binary values.
* StreamReader and StreamWriter – for reading and writing characters by using an encoding value to convert the characters to and from bytes.
* StringReader and StringWriter – for reading and writing characters to and from strings.
* TextReader and TextWriter – serve as the abstract base classes for other readers and writers that read and write characters and strings, but not binary data.
### Asynchronous I/O operations
* Reading or writing a large amount of data can be resource-intensive. You should perform these tasks asynchronously if your application needs to remain responsive to the user. With synchronous I/O operations, the UI thread is blocked until the resource-intensive operation has completed. Use asynchronous I/O operations when developing Windows 8.x Store apps to prevent creating the impression that your app has stopped working.
* The asynchronous members contain Async in their names, such as the CopyToAsync, FlushAsync, ReadAsync, and WriteAsync methods. You use these methods with the async and await keywords.
### Compression
* Compression refers to the process of reducing the size of a file for storage. Decompression is the process of extracting the contents of a compressed file so they are in a usable format. The System.IO.Compression namespace contains types for compressing and decompressing files and streams.
* ZipArchive – for creating and retrieving entries in the zip archive.
* ZipArchiveEntry – for representing a compressed file.
* ZipFile – for creating, extracting, and opening a compressed package.
* ZipFileExtensions – for creating and extracting entries in a compressed package.
* DeflateStream – for compressing and decompressing streams using the Deflate algorithm.
* GZipStream – for compressing and decompressing streams in gzip data format.
### Isolated storage
* Isolated storage is a data storage mechanism that provides isolation and safety by defining standardized ways of associating code with saved data. The storage provides a virtual file system that is isolated by user, assembly, and (optionally) domain. Isolated storage is particularly useful when your application does not have permission to access user files. You can save settings or files for your application in a manner that is controlled by the computer's security policy.
* Isolated storage is not available for Windows 8.x Store apps; instead, use application data classes in the Windows.Storage namespace. For more information, see Application data.
* IsolatedStorage – provides the base class for isolated storage implementations.
* IsolatedStorageFile – provides an isolated storage area that contains files and directories.
* IsolatedStorageFileStream - exposes a file within isolated storage.
### I/O operations in Windows Store apps
* The .NET for Windows 8.x Store apps contains many of the types for reading from and writing to streams; however, this set does not include all the .NET Framework I/O types.
* Types specifically related to file operations, such as File, FileInfo, Directory and DirectoryInfo, are not included in the .NET for Windows 8.x Store apps. Instead, use the types in the Windows.Storage namespace of the Windows Runtime, such as StorageFile and StorageFolder.
* Isolated storage is not available; instead, use application data.
* Use asynchronous methods, such as ReadAsync and WriteAsync, to prevent blocking the UI thread.
* The path-based compression types ZipFile and ZipFileExtensions are not available. Instead, use the types in the Windows.Storage.Compression namespace.
### I/O and security
* When you use the classes in the System.IO namespace, you must follow operating system security requirements such as access control lists (ACLs) to control access to files and directories. This requirement is in addition to any FileIOPermission requirements. You can manage ACLs programmatically. For more information, see How to: Add or Remove Access Control List Entries.
* Default security policies prevent Internet or intranet applications from accessing files on the user’s computer. Therefore, do not use the I/O classes that require a path to a physical file when writing code that will be downloaded over the Internet or intranet. Instead, use isolated storage for traditional .NET Framework applications, or use application data for Windows 8.x Store apps.
* A security check is performed only when the stream is constructed. Therefore, do not open a stream and then pass it to less-trusted code or application domains.
## How to: Write text to a file
* StreamWriter contains methods to write to a file synchronously (Write and WriteLine) or asynchronously (WriteAsync and WriteLineAsync).
* File provides static methods to write text to a file, such as WriteAllLines and WriteAllText, or to append text to a file, such as AppendAllLines, AppendAllText, and AppendText.
* Path is for strings that have file or directory path information. It contains the Combine method and, in .NET Core 2.1 and later, the Join and TryJoin methods, which allow concatenation of strings to build a file or directory path.
## How to: Read and write to a newly created data file
* The System.IO.BinaryWriter and System.IO.BinaryReader classes are used for writing and reading data other than character strings. The following example shows how to create an empty file stream, write data to it, and read data from it
* The example creates a data file called Test.data in the current directory, creates the associated BinaryWriter and BinaryReader objects, and uses the BinaryWriter object to write the integers 0 through 10 to Test.data, which leaves the file pointer at the end of the file. The BinaryReader object then sets the file pointer back to the origin and reads out the specified content.