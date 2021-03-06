#PicketLink Release Procedure#

##Pre-requisites##

Before releasing make sure your environment is properly configured as follows:

1. Make sure you have Git Flow installed and your local repository properly initialized.
    * https://github.com/nvie/gitflow/downloads
    * http://nvie.com/posts/a-successful-git-branching-model/
2. Make sure your ~/.m2/settings.xml is properly configure to allow you to publish the artifacts to JBoss Nexus.
    
3. Make sure you're able to upload the documentation to docs.jboss.org/picketlink/3/.

4. If you are in a fork, check if you have a remote repository configured as follows:

        git remote add upstream https://github.com/picketlink/picketlink

5. Check for uncommitted changes, before continuing.

##Release##

To release, simply run:  
      
      ./release.sh --snapshot <from snapshot version> --version <release version>

  This will perform the following steps:
  
  1. Update your master and develop branchs with the latest changes and sync them.
  3. Check if all depencies are available in central.
  4. Execute the maven-release-plugin in DryRun mode.
  5. Perform the release.
  6. Create a tag for the new version.
  7. Publish the artifacts into JBoss Nexus.
  8. Upload the documentation to docs.jboss.org.

###Post-Release steps###

####Closing the Staging repository in JBoss Nexus####

While you published your artifacts, they won't get automatically synced to the JBoss Community repository without a nod from you. You give the nod by promoting the staged artifacts through the Nexus web interface. 

Follow these steps to kick off the promotion:

1. Login to http://repository.jboss.org/nexus
2. Click on Staging Repositories link in the left hand navigation
3. Look for the staging activity with your username in the repository name with a status closed
4. Check the row and click Release or Drop

####Upload the distribution package to sourceforge####

TODO

####Update JDF BOMs with the new version####

It is important to update the JDF BOMs with the new version. Basically you should send a PR updating the following project:

* https://github.com/jboss-jdf/jboss-bom/tree/master/jboss-javaee-6.0-with-security

####Update PicketLink Site####

The PicketLink site must be update with the new release. The site is located at http://jboss.org/picketlink.

You need a valid account in order to make the changes. The administration panel is at http://jboss.org/author.

The following changes need to be done:

* Update the announcement on the Main Page to reflect the new version.
* Update the Download Page with the new version.

##If something goes wrong##

First, check the pre-requisites section for missed steps.

The release script generates a log file called pl-release.log. You can check it for more details about the release execution.

###Rollback###
You can always rollback the release in order to start again. For that use the following command:

    ./release.sh --version <release version> --rollback