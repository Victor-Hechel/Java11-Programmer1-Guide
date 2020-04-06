
# Understanding Java Technology and environment

## Describe Java Technology and the Java development

Right below are listed the main concepts about the Java Technology.

- **Plataform Independence:** After finishing a Java code we compile it to **bytecode**, which is what will actually be executed. The JVM (Java Virtual Machine) takes the generated **.class file** and runs it. The really advantage of this feature is the possibility to develop and run Java applications on any kind of plataform that has the JVM installed. Android and IPhone are example of enviroments where there is no JVM, so is not possible to run Java programs on them.

- **JRE:** Java Runtime Enviroment is a pack of class libraries and executables. It's only propuse is to run java application. We normally install it when there is no need to develop programs.

- **JDK:** Java Development Kit includes JRE and other developers tools. JDK is the kit used for Java developers to build their applications.

- **Development tools:** The applications included in the JDK are: Java compiler (javac), Java debugger, Class inspector (javap), documentation generator (javadoc) and others. It also comes with the JVM.

- **Java Platform configurations:** On Java 11, due the implementation of modules, is possible to build a small JRE image to run just the necessary modules on your application.

## Identify key features of the Java language

- **Object-Oriented:** Java is a Object-Oriented language and includes many principles like encapsulation, multiple inheritance of type (but not of state), polymorphism and others.

- **Huge standard library:** JRE includes a lot of libraries for the most different situations, such as File I/O, text processing, data structures, network features and so on...

- **Less complex:** One of the main principles of Java is to eliminate the complexity. That's the reason why the language doesn't have pointers, multiple class inheritance, operator overloading and such complex features.

- **Garbage collector:** Java includes a Garbage Collector, so developers don't need to concern about freeing resources from the memory. This process is performed on the background and doesn't have a high cost on performance.

- **Security:** Java programs can be runt with a security manager, which can be configured to allow only certain operations.

- **Multithreading:** Java tries to simplify the development of multithreading applications, but it doesn't parallel execution on his own.

- **High Performance:** Java interpreters are highly optimized and have the ability to watch blocks that are executed frequently, improving their performances.
