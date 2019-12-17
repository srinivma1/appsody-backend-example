# appsody-backend-example

   This writeup will show how to generate cloud native microservice applications based on appsody(IBM Cloud pak for applications)

## Prerequisites for Windows 10 Professional
 1. Minimum OS should be Windows 10 Professional.
 
 2. Docker for Windows 10 installed.
 
        https://docs.docker.com/docker-for-windows/install/
      
 3. Appsody installed 
 
        https://appsody.dev/docs/getting-started/installation/#installing-on-windows
        
        
 
 
## Getting started
### Configuring Appsody

**Note**: The use of the public Kabanero Collection Hub is only for the purposes of this guide. It is recommended that you make a private copy of the Kabanero Collection Hub and use it in the same way. However, this demonstration does not require that you make your own copy.

Add your Kabanero index to the Appsody CLI. The following example uses the public index for Kabanero Version 0.3.0.

To check the repositories that Appsody can already access, run the following command:

    appsody repo list
You see an output similar to the following example:

    NAME        URL
    *incubator https://github.com/appsody/stacks/releases/latest/download/incubator-index.yaml
    Next, run the following command to add the Kabanero index:

    appsody repo add kabanero https://github.com/kabanero-io/collections/releases/download/0.3.0/kabanero-index.yaml
    
Check the existing repositories to see that it was added:

    NAME        URL
    *incubator https://github.com/appsody/stacks/releases/latest/download/incubator-index.yaml
    kabanero    https://github.com/kabanero-io/collections/releases/download/0.3.0/kabanero-index.yaml
    
In this example, the asterisk (*) shows that incubator is the default repository. Run the following command to set the Kabanero index as the default repository:

    appsody repo set-default kabanero
    
Check the existing repositories to see that it was updated:

      NAME        URL
      incubator  https://github.com/appsody/stacks/releases/latest/download/incubator-index.yaml
      *kabanero   https://github.com/kabanero-io/collections/releases/download/0.3.0/kabanero-index.yaml
      Recommendation: In enterprise settings, when a solution architect creates application stacks with technology choices that are in a private Collection Hub, it’s best to remove incubator from the list. These Appsody stacks are not supported by the Kabanero application cluster.

Run the following command to remove the incubator repository:

    appsody repo remove incubator
Check the existing repositories to see the change:

      NAME             URL
      *kabanero        https://github.com/kabanero-io/collections/releases/download/0.3.0/kabanero-index.yaml
Your Appsody CLI is now configured to use the Kabanero Collections. Next, you need to initialize your project.



## Initializing your project
First, create a directory that will contain the project:

      mkdir -p ~/projects/simple-microprofile
      cd ~/projects/simple-microprofile
      
Run the following command to initialize the project with the Appsody CLI:

      appsody init java-microprofile
      
The output from the command varies depending on whether you have an installation of Java and Maven on your system. If Java and Maven are installed on your system, you see an output similar to the following example:

      [InitScript] [INFO] -------------------< dev.appsody:java-microprofile >--------------------
      [InitScript] [INFO] Building java-microprofile 0.2.11
      [InitScript] [INFO] --------------------------------[ pom ]---------------------------------
      [InitScript] [INFO]
      [InitScript] [INFO] --- maven-enforcer-plugin:3.0.0-M2:enforce (enforce-versions) @ java-microprofile ---
      [InitScript] [INFO] Skipping Rule Enforcement.
      [InitScript] [INFO]
      [InitScript] [INFO] --- maven-install-plugin:2.4:install (default-install) @ java-microprofile ---
      [InitScript] [INFO] Installing /Users/myuser/projects/simple-microprofile/.appsody_init/pom.xml to /Users/myuser/.m2/repository/dev/appsody/java-microprofile/0.2.11/java-microprofile-0.2.11.pom
      [InitScript] [INFO] ------------------------------------------------------------------------
      [InitScript] [INFO] BUILD SUCCESS
      [InitScript] [INFO] ------------------------------------------------------------------------
      [InitScript] [INFO] Total time:  0.648 s
      [InitScript] [INFO] Finished at: 2019-09-13T10:17:55+01:00
      [InitScript] [INFO] ------------------------------------------------------------------------
      Successfully initialized Appsody project
      
If Java and Maven are not installed on your system, you see an output similar to the following example:

      [InitScript] Unable to find any JVMs matching version "(null)".
      [InitScript] No Java runtime present, try --request to install.
      [InitScript] Unable to find a $JAVA_HOME at "/usr", continuing with system-provided Java...
      [InitScript] No Java runtime present, requesting install.
      [Warning] The stack init script failed: exit status 1
      [Warning] Your local IDE may not build properly, but the Appsody container should still work.
      [Warning] To try again, resolve the issue then run `appsody init` with no arguments.
      
Your project is now initialized.

## Understanding the project layout
For context, the following image displays the structure of the project that you’re working on:

Project structure


It contains the following artifacts:

      StarterApplication.java, a JAX-RS Application class

      server.xml, an Open Liberty server configuration file

      index.html, a static HTML file

      pom.xml, a project build file
      
## Workaround if docker destop not accepting credentials using Azure Active Directory for sharing files

If Docker Desktop does not accept your AAD user and password in the "Shared Drives" tab of the Docker "Settings" panel, or you just do not have the password for your user, the only known workaround at this time is to use a separate, local, Windows account to handle the drive sharing and file permissions.

  https://appsody.dev/docs/docker-windows-aad/#workaround-for-windows-10-enterprise-secured-with-azure-active-directory

## Running the Appsody development environment

Run the following command to start the Appsody development environment:

        appsody run
        
The Appsody CLI launches a local Docker image that contains an Open Liberty server that hosts the microservice. After some time, you see a message similar to the following example:

    [Container] [INFO] [AUDIT   ] CWWKF0011I: The defaultServer server is ready to run a smarter planet. The defaultServer server started in 20.235 seconds.
This message indicates that the server is started and you are ready to begin developing your application.

## Creating and updating the application
Now you can create your business logic. Typically, you put your business logic in a JAX-RS resource. First, you need to add a REST endpoint.

Create a StarterResource.java class in the src/main/java/dev/appsody/starter directory. Open the file, populate it with the following code, and save it:

          package dev.appsody.starter;
          import javax.ws.rs.GET;
          import javax.ws.rs.Path;
          @Path("/resource")
          public class StarterResource {
              @GET
              public String getRequest() {
                  return "StarterResource response";
              }
          }
After you save, the source compiles and the application updates. You see messages similar to the following example:

      [Container] [INFO] [AUDIT   ] CWWKT0017I: Web application removed (default_host): http://85862d8696be:9080/
      [Container] [INFO] [AUDIT   ] CWWKZ0009I: The application starter-app has stopped successfully.
      [Container] [INFO] [AUDIT   ] CWWKT0016I: Web application available (default_host): http://85862d8696be:9080/
      [Container] [INFO] [AUDIT   ] CWWKZ0003I: The application starter-app updated in 0.988 seconds.
      
The resource that you just added is available at the starter/resource URL path. Go to the http://localhost:9080/starter/resource URL to see the following resource response:

## StarterResource response
Try changing the message in the StarterResource.java file, saving, and refreshing the page. You’ll see that it takes only a few seconds for the change to take effect.

## Testing the application
If you are building an application that is composed of microservices, you need to test within the context of the overall system. First, test your application and perform unit testing in isolation. To test the application as part of the system, deploy the system and then the new application.

You can choose how you want to deploy the system and application. If you have adequate CPU and memory to run MiniShift, the application, and the associated services, then you can deploy the application on a local Kubernetes that is running on your computer. Alternatively, you can enable Docker Desktop for Kubernetes, which is described in the Prerequisites section of the guide.

You can also deploy the system, application, and the associated services in a private namespace on a development cluster. From this private namespace, you can commit the microservices in Git repositories and deploy them through a DevOps pipeline, not directly to Kubernetes.

## Testing locally on Kubernetes
After you finish writing your application code, the Appsody CLI makes it easy to deploy directly to a Kubernetes cluster for further local testing. The ability to deploy directly to a Kubernetes cluster is valuable when you want to test multiple microservices together or test with services that the application requires.

Ensure that your kubectl command is configured with cluster details, and run the following command to deploy your application:

      appsody deploy
      
This command builds a new Docker image that is optimized for production deployment and deploys the image to your local Kubernetes cluster. After some time you see a message similar to the following example:

Deployed project running at http://localhost:30262
Run the following command to check the status of the application pods:

     kubectl get pods
You see an output similar to the following example:

    NAME                                  READY    STATUS   RESTARTS   AGE
    appsody-operator-859b97bb98-htpgw      1/1     Running   0         3m2s
    simple-microprofile-77d6868765-xkcpk   1/1     Running   0         31s
    
The pod that is related to your deployed application is similar to the following pod:

    simple-microprofile-77d6868765-xkcpk   1/1     Running   0         31s
    
After the simple-microprofile pod starts, go to the URL that was returned after you ran the appsody deploy command, and you see the Appsody microservice splash screen. To see the response from your application, point your browser to <URL_STRING>/starter/resource, where <URL_STRING> is the URL that was returned. For example, the http://localhost:30262 URL was returned in the previous example. Go to the http://localhost:30262/starter/resource URL to see the deployed application response.

Use the following command to stop the deployed application:

      appsody deploy delete
After you run this command, and the deployment is deleted, you see the following message:

Deployment deleted
 
