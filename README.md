Download Link: https://assignmentchef.com/product/solved-csci-3753-operating-systems-programming-assignment-three-solution
<br>



<strong>Goal: </strong>Using Pthreads to code a DNS name resolution engine

<h1>1             Introduction</h1>

In this assignment you will develop a multi-threaded application that resolves domain names to IP addresses, similar to the operation performed each time you access a new website in your web browser. The application is composed of two sub-systems, each with one thread pool: requesters and resolvers. The sub-systems communicate with each other using a bounded queue.

This type of system architecture is referred to as a Producer-Consumer architecture. It is also used in search engine systems, like Google. In these systems, a set of crawler threads place URLs onto a queue. This queue is then serviced by a set of indexer threads which connect to the websites, parse the content, and then add an entry to a search index. Refer to Figure 1 for a visual description.

Figure 1: System Architecture

<h1>2             Description</h1>

<h2>2.1           Input: Name Files</h2>

Your appliction will take as input a set of name files. Names files contain one hostname per line. Each name file should be serviced by a single requester thread from the requester thread pool.

<h2>2.2           Requester Threads</h2>

The requester thread pool services a set of name files, each of which contains a list of domain names. Each name that is read from each of the files is placed on a FIFO queue. If a requester thread tries to write to the queue but finds that it is full, it should block until a space opens up in the queue.

<h2>2.3           Resolver Threads</h2>

The second thread pool is comprised of a set of THREAD MAX resolver threads. The resolver thread pool services the FIFO queue by taking a name off the queue and querying its IP address. After the name has been mapped to an IP address, the output is written to a line in the results.txt file in the following format:

www.google.com,74.125.224.81

If a resolver thread tries to read from the queue but finds that it is empty, it should block until there is a new item in the queue or all names in the input files have been serviced.

<h2>2.4           Synchronization and Deadlock</h2>

Your application should synchronize access to shared resources and avoid deadlock or busy wait. You should use mutexes and condition variables to meet this requirement. There are at least two shared resources that must be protected: the queue and the output file. Neither of these resources is thread-safe by default.

<h2>2.5           Ending the Program</h2>

Your program must end after all the names in each file have been serviced by the application. This means that all the hostnames in all the input files have received a corresponding line in the output file.

<h1>3             What’s Included</h1>

Some files are included with this assignment for your benefit. You are not required to use these files, but they may prove helpful.

<ol>

 <li><strong>h </strong>and <strong>queue.c</strong>: These two files implement the FIFO queue data structure. The queue accepts pointers to arbitrary types. You should probably use the queue to store pointers to C-strings of hostnames. The requester threads should push these hostnames into the queue, and the resolver threads should obtain hostnames from the same queue.</li>

</ol>

Please consult the <em>queue.h </em>header file for more detailed descriptions of each available function.

<ol start="2">

 <li><strong>c</strong>: This program runs a series of tests to confirm that the queue is working correctly. You may use it as an example of how to use the queue, or to test the functionality of the queue code that you are provided.</li>

 <li><strong>c </strong>and <strong>util.h</strong>: These two files contain the DNS lookup utility function. This function abstracts away a lot of the complexity involved with performing a DNS lookup. The function accepts a hostname as input and generates a corresponding dot-formatted IPv4 IP address string as output.</li>

</ol>

Please consult the <em>util.h </em>header file for more detailed descriptions of each available function.

<ol start="4">

 <li><strong>input/names*.txt </strong>This is a set of sample name files. They follow the same format as mentioned earlier. Use them to test your program.</li>

 <li><strong>results-ref.txt </strong>This result file is a sample output of the IPs for the hostnames from the <em>txt </em>file.</li>

 <li><strong>c </strong>This program represents an un-threaded solution to this assignment. Feel free to use it as a starting point for your program, or as a reference for using the utility functions and performing file I/O in C.</li>

 <li><strong>pthread-hello.c </strong>A simple threaded “Hello World” program to demonstrate basic use of the pthread library.</li>

 <li><strong>Makefile </strong>A GNU Make makefile to build all the code listed here.</li>

</ol>

<h1>4             Additional Specifications</h1>

Many of the specifications for your program are embedded in the descriptions above. This section details additional specifications you must adhere to.

<h2>4.1           Program Arguments</h2>

Your executable program should be named “multi-lookup”. When called, it should interpret the last argument as the file path for the file to which results will be written. All proceeding arguments should be interpreted as input files containing hostnames in the aforementioned format.

An example call involving three input files might look like: multi-lookup names1.txt names2.txt names3.txt result.txt

<h2>4.2           Limits</h2>

If necessary, you may impose the following limits on your program. If the user specifies input that would require the violation of an imposed limit, your program should gracefully alert the user to the limit and exit with an error.




<strong>MAX INPUT FILES</strong>: 10 Files (This is an optional upper-limit. Your program may also handle more files, or an unbounded number of files, but may not be limited to less than 10 input files.)

<ul>

 <li><strong>MAX RESOLVER THREADS</strong>: 10 Threads (This is an optional upper-limit. Your program may also handle more threads, or match the number of threads to the number of processor cores.)</li>

 <li><strong>MIN RESOLVER THREADS</strong>: 2 Threads (This is a mandatory lower-limit. Your program may handle more threads, or match the number of threads to the number of processor cores, but must always provide at least 2 resolver threads.)</li>

 <li><strong>MAX NAME LENGTH</strong>: 1025 Characters, including null terminator (This is an optional upper-limit. Your program may handle longer names, but you may not limit the name length to less than 1025 characters.)</li>

 <li><strong>MAX IP LENGTH</strong>: INET6 ADDRSTRLEN (This is an optional upper-limit. Your program may handle longer IP address strings, but you may not limit the name length to less than INET6 ADDRSTRLEN characters including the null terminator.)</li>

</ul>

<h2>4.3           Error Handling</h2>

You must handle the following errors in the following manners:

<ul>

 <li><strong>Bogus Hostname</strong>: Given a hostname that can not be resolved, your program should output a blank string for the IP address, such that the output file contines the hostname, followed by a comma, followed by a line return. You should also print a message to stderr alerting the user to the bogus hostname.</li>

 <li><strong>Bogus Output File Path</strong>: Given a bad output file path, your program should exit and print an appropriate error to stderr.</li>

 <li><strong>Bogus Input File Path</strong>: Given a bad input file path, your program should print an appropriate error to stderr and move on to the next file.</li>

</ul>

All system and library calls should be checked for errors. If you encounter errors not listed above, you should print an appropriate message to stderr, and then either exit or continue, depending upon whether or not you can recover from the error gracefully.

<h1>5             External Resources</h1>

You may use the following libraries and code to complete this assignment, as well as anything you have written for this assignment:

<ul>

 <li>Any functions listed in util.h</li>

 <li>Any functions listed in queue.h</li>

</ul>

Any functions in the C Standard Library

<ul>

 <li>Standard Linux pthread functions</li>

 <li>Standard Linux Random Number Generator functions</li>

 <li>Standard Linux file i/o functions</li>

</ul>

If you would like to use additional external libraries, you must clear it with the TA first. You will not be allowed to use pre-existing thread-safe queue or file i/o libraries since the point of this assignment is to teach you how to make non-thread-safe resources thread-safe.

<h1>6             What You Must Provide</h1>

To receive full credit, you must submit the following items to Moodle by the due date. Please combine the files into a single zip or tar archive.

<ul>

 <li><strong>multi-lookup.c</strong>: Your program, conforming to the above requirements</li>

 <li><strong>multi-lookup.h</strong>: A header file containing prototypes for any function you write as part of your program.</li>

 <li><strong>Makefile</strong>: A makefile that builds your program as the default target. It should also contain a ’clean’ target that will remove any files generated during the course of building and/or running your program.</li>

 <li><strong>README</strong>: A readme describing how to build and run your program.</li>

 <li>Any additional files necessary to build or run your program</li>

</ul>

<h1>7             Extra Credit</h1>

There are a few options for receiving extra credit on this assignment. Completion of each of the following items will gain you 5 points of extra credit per item. In no case will the maximum score on this assignment exceed 110/100.

<ul>

 <li><strong>Multiple IP Addresses</strong>: Many hostnames return more than a single IP address. Add support for listing an arbitrary number of addresses to your program. These addresses should be printed to the output file as additional comma-separated strings after the hostname. For example:</li>

</ul>

www.google.com,74.125.224.81,76.125.232.80,75.125.211.70

You may find it necessary to modify code in the util.h and util.c files to add this functionality. If you do this, please maintain backwards compatibility with the existing util.h functions. This is most easily done by adding new function instead of modifying the existing ones.

<strong>IPv6 Support and Testing</strong>: Add support for IPv6 IP addresses and find an IPv6 aware environment where you can test this support. Since IPv6 is relatively new, finding an environment for testing this support is probably harder than adding it. You must be able to demonstrate IPv6 support during your grading session to receive credit for this item. You may find it necessary to modify code in util.h and util.c to complete this item. If you do so, please maintain backward compatibility with the existing code.

<ul>

 <li><strong>Matching Number of Threads to Number of Cores</strong>: Make your program dynamically detect the number of cores available on a system and set the number of resolver threads to take into account the number of cores.</li>

 <li><strong>Full-Loop Lookups</strong>: Make each requester thread query the output file every 250ms to detect when each of its requests have been filled. Requester threads should print a message to the user with the IP address of each hostname, and should not exit until all of each threads requests have been satisfied.</li>

 <li><strong>Benchmarks</strong>: Determine the ideal number of resolver threads for a given processor core count. Provide benchmark data backing up your determination. Include this documentation in your README.</li>

</ul>

<h1>8             Grading</h1>

Like all CSCI 3753 assignments, 40% of you grade will be based on providing a working solution. To received full credit your program must:

<ul>

 <li>Meet all requirements elicited in this document</li>

 <li>Build with “-Wall” and “-Wextra” enabled, producing no errors or warnings.</li>

 <li>Run without leaking any memory, as measured using valgrind</li>

</ul>

The other 60% of your grade will be determined via your grading interview where you will be expected to explain your code and answer questions regarding it and the concepts related to this assignment. This includes adhering to good coding style practices.

To verify that you do not leak memory, TAs may use <em>valgrind </em>to test your program. To install <em>valgrind</em>, use the following command:

sudo apt-get install valgrind

And to use <em>valgrind </em>to monitor your program, use this command:

valgrind ./pa2main text1.txt text2.txt …… textN.txt results.txt

Valgrind should report that you have freed all allocated memory and should not produce any additional warnings or errors.

You can write your code in any environment you like. But you have to make sure that your programs can be compiled and executed in the Virtual Machine that has been provided for this class or on the CSEL machines.




<h1>9             References</h1>

Refer to your textbook and class notes on the Moodle for descriptions of producer/consumer and reader/writer problems and the different strategies used to solve them.

The Internet is also a good resource for finding information related to solving this assignment.