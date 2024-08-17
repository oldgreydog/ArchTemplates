# ArchTemplates

At the moment, this is a pathfinder project that I hope will show a way to use code generation for creating the base working code for many of the parts that are needed for applications, both front-end and back-end.  When I started on this, I thought I would be able to create template sets for a range of technologies in two or three languages, but the more I looked at what people were doing out there, it became ever more apparent that there were too many permutations for me to handle.

Given that, to come anywhere close to that vision by myself, I would have had to make choices about things that I don't feel comfortable making by myself.  Charging in that direction would have probably created things that wouldn't have made anyone happy and probably lead to it all being thrown out.

In the end, I decided to use some of the templates that I had been using myself as the core around which I would try to figure out ways to manage the project creation and generation.   That could then be used to support the development of useful templates sets with the input of other people.

The templates that you see here are for access to PostgreSQL in java.  They depend on libraries that I don't expect you to be interested in so I'm just providing the compiled jars in the ArchTemp_Example zip file in the releases so that it will all compile and the tests will run.

The thing you should pay attention to in this initial release is how I set this up so that you only need the code generator and a few config files to make things work.  When I started working on this, I thought that I might have to build a project setup app that would be able to interpret the root project_setup.xml file to correctly set up only the technologies and languages that the user wanted.  But that *really* didn't sit well with me, so I wanted to find a simpler(?) solution.

What I settled on does the job with just the code generator and batch files.  You need to keep in mind that the template tree does not have to match the generated output tree.  That let's us use the template tree to organize things by concept (i.e. technology/language) but the generated tree can be in project management/IDE format.

# Example

To make it easier to see this at work, I've included a zip file in the release that contains the initial set of files you need to setup and generate an example project.  That includes a db schema definition that you would normally need to create yourself, but I provide a filled example here to make it as easy as possible.

To run the example, you need:

	- Linux:  This is one that I have to apologize for.  I only have batch files for Linux.  I haven't had a Windows machine in years, so I don't have a way to test Windows batch files and I don't want to try to create them without being able to test them.  Of course, the real problem is that some of the batch files are generated, so we'd have to have Windows versions of those templates, too.  Using a Linux virtual machine of your choice is probably the easiest solution.
	- Java: DO NOT USE ORACLE JAVA!!  If your company doesn't have a license for Oracle java, you could expose the company to a HUGE financial liability if you install Oracle's official version of java.  Instead, use OpenJKD.  It is the open source base for the language that Oracle builds its java release on top of.  It comes in all of the Linux distributions and can be downloaded for Windows.
	- The zip files:  Grab the "Source Code" and ArchTemp_Example zip files on the project releases page.  Create a directory and unzip both of those files into it.
	- Set the path in the config file:  In the ArchTemp_Example directory, open the project_setup.xml and set the value for sharedProjectPath.  This needs to be set to the full path of the directory that holds the ArchTemplates and ArchTemp_Example directories.
	- Run the batch files in the ArchTemp_Example directory:
		* ./setup_project
		* ./generate_all
		* ./build_all

If you want to run the tests, you need access to an instance of PostgreSQL.  Their documentation and/or search will get you to a pretty easy setup for a local instance if you're inclined to a local install.  You could also use a VM or you can get it in a container.  For a local instance, you can get away with just setting up the admin user and using that for the tests.  If you are going to use an instance installed elsewhere, you'll have to get that instance's admin to give you a user login that has database creation rights.

Once you have your db access, you need to:

	- Add the user ID/password to the config file:  Go into the coredbUnitTestConfig.xml config file in ArchTemp_Example/config/coredb/testing and add the user ID and password in two places.  The first is in the "root.sqlDB.coredb_server" block and the other is in "root.sqlDB.coredb".  There are two because the code needs a db connection that points to the admin login so that it can add and remove the test db instances.  The second is the "user" connection that the code uses to do the regular db create/read/update/delete calls.  Depending on where your DB server instance is located, you might also need to alter the network address and/or port number in the "url" config value for both of the connection sections.
	- Run the testing batch file:  ./run_testing_gui

run_testing_gui will open a Java Swing testing GUI that I created many years back to have a way to run single tests out of a test suite so that I could easily use it to drive debugging of a particular problem over and over.  To use it, you need to highlight one or more tests and/or node(s)/folder(s) and click "Activate Tests" and then "Execute Tests".

For this example code, the best way to run it is to maximize the app, open the "folders" of tests by clicking on the spinners next to "coredb" then "VO Member Validation Tests" and "DAO Function Tests".  Then click on the "root" folder and hit "Activate Tests" and then "Execute Tests".  The icons next to the folders and test cases will change color as they are activated, waiting to be run and then finished (i.e. pass or fail).  If everything is set up correctly, you should get green "pass" icons from top to bottom.


