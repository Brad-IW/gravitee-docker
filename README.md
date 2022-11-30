# Gravitee Docker

## Installation Guide

This guide assumes that [Docker](https://docs.docker.com/get-docker/) has already been installed and initialised. 

1. Clone this repo and extract the files.
2. Open a terminal and navigate to the gravitee-docker folder.
3. Run `docker compose up` 
    
    Once the compose file has finished its start up procedure the GUI will be accessible on port 8084.


## Test Plan

This optional test plan will take you thruogh the process of creating a simple API using the Gravitee web GUI. The plan assumes that the actions below are performed on a local machine running the docker containers, however this can easily be changed by swapping all instances of localhost to the ip address of the machine running docker.

### Basic API Example

In this section you will create a simple API which returns a response from [httpbin](http://httpbin.org/).

1. Open the Gravitee Management Portal by opening `localhost:8084` in your web browser of choice.
2. Login to the portal with the username `admin` and the password `admin`
3. On the left side bar select APIs, then press Add Api. Choose the Continue in the Wizard option.
   Give your API a name, in this example we will call it MyAPI.
   Choose a version for the API, for example v1
   Add a description e.g. My new API.
   Set the context path, this is the route which the API will be accessible from. For example if we set the path to `/myapi` the API will be accessible from `localhost:8082/myapi`
4. Press next and choose a backend.
   The backend is the location where the API lives. In this example we will set the backend to `http://httpbin.org/anything`. This means that a request sent to `localhost:8082/myapi` will be forwarded to the API running at `http://httpbin.org/anything`
5. Press next and setup a basic plan.
   The plan requires three fields
    - The plan name, e.g. Basic Plan
    - The description, e.g. My plan description.
    - The security type. In this example we will set the security type to keyless, so we can access the API without a key.
    - Any rate limiting or quotas can be optionally set from this page too.
6. Press next, then hit skip on the import file doc page.
7. Finally press the CREATE AND START THE API button to start the API.
   If done correctly the API will now be accessible by opening `localhost:8082/myapi` in your web browser, or running `curl localhost:8082/myapi` (`curl 127.0.0.1/myapi` on Windows) in your terminal. If all steps were done correctly this should return a JSON response which echos back the information sent in the request.