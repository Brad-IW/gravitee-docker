# Gravitee Docker

## Installation Guide

This guide assumes that [Docker](https://docs.docker.com/get-docker/) has already been installed and initialised. 

1. Clone this repo and extract the files.
2. Open a terminal and navigate to the gravitee-docker folder.
3. Run `docker compose up` 
    
    Once the compose file has finished its start up procedure the GUI will be accessible at `localhost:8084`. 
    
4. After ensuring that the has container built and ran successfully, that and the GUI is available and working type `CTRL + C` or `COMMAND + C` on Linux and Mac to stop the containers.
5. Run `docker compose up -d` to run the container in the background.


## Test Plan

This optional test plan will take you thruogh the process of creating a simple API using the Gravitee web GUI. The plan assumes that the actions below are performed on a local machine running the docker containers, however this can easily be changed by swapping all instances of localhost to the ip address of the machine running docker.

### Basic API Example

In this section you will create a simple API which returns a response from [httpbin](http://httpbin.org/).

1. Open the Gravitee Management Portal by opening `localhost:8084` in your web browser of choice.
2. Login to the portal with the username `admin` and the password `admin`
3. On the left side bar select APIs, then press Add Api. Choose the Continue in the Wizard option.
   - Give your API a name, in this example we will call it MyAPI.
   - Choose a version for the API, for example v1
   - Add a description e.g. My new API.
   - Set the context path, this is the route which the API will be accessible from. For example if we set the path to `/myapi` the API will be accessible from `localhost:8082/myapi`

4. Press next and choose a backend.
   
   The backend is the location where the API lives. In this example we will set the backend to `http://httpbin.org/anything`. This means that a request sent to `localhost:8082/myapi` will be forwarded to the API running at `http://httpbin.org/anything`

5. Press next and setup a basic plan.
   
   The plan requires three fields:
    - The plan name, e.g. Basic Plan
    - The description, e.g. My plan description.
    - The security type. In this example we will set the security type to keyless, so we can access the API without a key.
    - Any rate limiting or quotas can optionally be set from this page too.

6. Press next, then click skip on the import file doc page.
7. Finally press the CREATE AND START THE API button to start the API.

   If done correctly the API will now be accessible by opening `localhost:8082/myapi` in your web browser, or running `curl localhost:8082/myapi` (`curl 127.0.0.1/myapi` on Windows) in your terminal. If all steps were done correctly this should return a JSON response which echos back the information sent in the request.

### Adding An API Key 

First we need to delete our original plan.

1. Select APIs on the left panel, and click on the API we created in the last step (MyAPI).
2. Click the plans button and find the plan we created in the last step (Basic Plan).
3. Press the X button to close the plan.
4. Type the name of the plan in the name field and click YES, CLOSE THIS PLAN.

Now that the original plan has been deleted we can create a new plan.

5. Press the + button to open the plan creation window.
   - Give the plan a name, e.g. Secure Plan.
   - Add a description to the plan, for example A secure plan.
   - Click the Auto validate subscription button under the Subscriptions section.
6. Press next, then under the authentication dropdown select API Key.
   Make sure to enable the Propogate API Key to upstream API option.
7. Press next again, then save.
8. Back on the plans page, press the cloud button to publish the plan, then hit the deploy your API button at the top of the screen.

The API has now been changed to require an API Key for all requests. Accessing the API once again at `localhost:8082/myapi` will now return a 401 Unauthorized response when trying to access the API without a key.

To get an API key we will create an application for the API.

9. On the left panel select the Applications button.
10. In the top right press Add Application to begin creating an application.
    - Give the application a name, e.g. MyApp
    - Set the description for the application, for example My app description 
11. Press next, then skip.
12. In the select an API to subscribe field, choose the API we previously created.
13. The plan we previously created will now be displayed in the center of the screen. Press the subscribe button which has now shown up.
14. Click next, then press the CREATE THE APPLICATION button.

Now that the application has been created we can generate an API key.

15. Go back to the applications page by selecting the applications button on the left panel.
16. Click the application we just created (MyApp)
17. Under the MyApp section on the left panel, select the subscriptions button.
18. Select MyAPI under the subscriptions tab.

From here we now have access to the API Key. The key can be used by either adding X-Gravitee-Api-Key to the header along with the key, or by appending api-key to the request parameters.

Accessing `localhost:8082/myapi?api-key=KEY` (where `KEY` is the key which was generated in the last step) in your web browser will return the JSON echo response from the previous basic API example. Providing an incorrect key or no key at all will continue to return the unauthorized response.

Alternatively we can use curl by running `curl localhost:8082/myapi -H "X-Gravitee-Api-Key: KEY"` (where `KEY` is the key which was generated in the last step) on a linux machine. This will pass the key through as a header and should return the same response as above.
