# HOW TO: Extract a service module from a web module
## Introduction
This HOW-TO explains how you can split an existing web module (with server actions and data), into a separate 
server module containing the data and the server actions manipulating the data.

It is a best practice to separate the application core (data + backend server actions) and the UI.  When you are dealing with an 
application where UI and data are in the same web module, then you can follow the steps below the seperate the two, whilst not 
losing any data in the process.

This information is based on the following Outsystems article: [Convert to Services](https://success.outsystems.com/Documentation/11/Developing_an_Application/Reuse_and_Refactor/Convert_to_Services).

## Steps
1. Open your web module that contains both the data and the UI.
1. Go over all your screen actions and look for those that directly call any of the Create-, CreateOrUpdate-, Update- or Delete-methods 
of your entities.
    1.  Extract the call to the Create-, CreateOrUpdate-, Update- or Delete-method into a Server Action.
1. Go to `Menu > Clone` to clone the module. Rename it to <Your_Module_Name>_UI.
1. Move the cloned module to your original application:
    1. Go to the "Independent Modules" application.
    1. Hover over the <Your_Module_Name>_UI module and click "Move To...".
    1. Select the application that contains your original module.
1. Open your original module, remove all UI-related code (screens, images, themes, session variables, ...).
1. Make sure all entities and server actions that are used in the UI are public.  Put "Expose Read Only" to "Yes" on the entities.
1. You can remove server actions that are UI-related.
1. Go to `Menu > Convert to Service Module...` to convert the original module to a Service Module.  
Remove any remaining UI elements Outsystems complains about.
1. Publish the module.
1. Go to the <Your_Module_Name>_UI module that you cloned.
1. Remove all elements that remained in the original module (that we converted to a Service Module).  
Leave all elements you deleted from the original module.  There should now be no overlap in the two modules.  
Each element is now either in the service module or the web module, not in both. You should now have a lot of *TrueChange* errors.
1. Add the original module (now a Service Module) as a dependency to the _UI module.  Check all elements that are used in the UI module.  
This should fix your *TrueChange* errors.
1. Congratulations, you have extracted a service module from your web module!  You have created a new UI module, so that means you have 
also created new roles for this new UI module.  Your existing users will now need these new roles instead of the old ones.
