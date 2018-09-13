# Lesson Eight: Creating a Sample Application via the Web UI

We created a user in the previous lesson, cleverly named `user`. Now we will deploy a sample application that leverages an existing container image and also builds another container via source code. For this example, we’re adapting a lesson from https://learn.openshift.com. 

While we can create applications from the command line, we will show how to use the web interface. Because of the environment we’re using, you will open a graphical console on the cloud provider which includes a web browser that we’ll be using for this lab. You will use the same student credentials as you did when using ssh to reach your workstation.

The page that details your student information has a link to "Control Center". (Just below your Student number.) This link gives you additional access to the training environment. Find the box for "WORKSTATION" and click "CONSOLE". You will be taken to a graphical console of the workstation. Log in with your student password provided by your instructor. 

After you’ve logged into the graphical console of the workstation, navigate to “Applications”, then “Firefox”. Once in Firefox, enter the url: 
```
https://master.example.com:8443
```
Log in with the `user` account you created earlier and perform the following steps:

Click the blue “Create Project” button and name the project *myproject*. The other fields are optional. After creating the project, click the blue *myproject* link.

Next click “Deploy Image”. This dialog allows us to deploy existing containers. Select the “Image Name” radio button and input: *openshiftroadshow/parksmap-katacoda:1.0.0*. Press return, then the blue “Deploy” button on the bottom right. 

Next click “Continue to Project Overview”. You’ll see an overview of *myproject* and can interact with the number of containers running, among many other controls. From this view, you can increase the number of running containers, view logs in real time, and take many other actions.

In order to interact with your application outside of the OpenShift cluster itself, you need to create a route. In the overview, ensure the arrow to the left of “parksmap-katacoda” is expanded. Click “create route” and accept the defaults. You’ll see a URL that you can click within the overview.

When you click the route, you should see a map. 

We will now create a container from existing source code, also called “S2I” or “source to image”. This container will be the backend for our map.

Click back to the Console tab. Click “Add to Project” in the upper right corner, then “Browse Catalog”. Find and select “Python”. Ensure the “Application Name” is *nationalparks-katacoda* and the “Git Repository” is:
```
https://github.com/openshift-roadshow/nationalparks-katacoda
```
Continue to the overview where you can watch the build progress.

Once the build completes, you can click on the first route you created earlier to see your map populated.

