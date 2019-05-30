# Fitnesse_WikiFramework
Fitnesse framework for testing Cognitive Solutions applications

# Set Up
As all paths in the framework are relative it can be checked out to any directory.
The following directories have been excluded from the repository:

* FitNesseRoot/files/
* FitNesseRoot/FitNesse/ComponentsAndLicenses/
* FitNesseRoot/FitNesse/FullReferenceGuide/
* FitNesseRoot/FitNesse/ReleaseNotes/
* FitNesseRoot/FitNesse/SuiteAcceptanceTests/
* FitNesseRoot/FitNesse/SuiteFitAcceptanceTests/
* FitNesseRoot/FitNesse/UserGuide/
* TemplateLibrary/

They are not necessary for the functioning of the framework.
However, you are free to add them to your local repository. The git ignore settings will mean they are not pushed to the remote repository.
The git ignore settings will also exclude test output files and the zip files which FitNesse creates when versioning.

To start Fitnesse just run StartFitnesse.bat on Windows or StartFitnesse.sh on Linux (you'll need to make it executable first).
By default Fitnesse will listen on port 9090 (change as you see fit). In your browser open 'localhost:9090' and click on the Cognitive Cities Suite of Suites link.
The wiki page below should be updated locally for your environment:

* FitNesse.CognitiveCitiesSuiteOfSuites.TestSuites.IocSuite.IocServicesSuite.SetUpVariables

It contains references to various environment-specific parameters (host names, user names, passwords, etc), along with several other variables used throughout the IOC test suites.

## Remote file copying
Some of the csv datasource tests copy test files from the local server to the application node. If you want to run these tests in your environment you will need to append the public key in './testfilfes/fitnesse_keyfile.ppk' to '/root/.ssh/authorized_keys' on the application node (whose hostname is defined by SetUp variable 'APPSERVER')

# Update procedure

* Create a new branch on the remote repository (or locally using 'git checkout -b <branch_name>')
* Work with the branch locally using the git command line.
* Push changes to remote repository when ready
* On the github web interface create a pull request to merge the branch changes to master, after review/approval by one or more collaborators.
* For more details on git hub command line see here: https://git-scm.com/book/en/v2)
