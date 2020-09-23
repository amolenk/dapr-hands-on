# dapr hands-on - Assignment 5 - Add secrets

## Assignment goals

In order to complete this assignment, the following goals must be met:

- The `GetVehicleDetails` method of the `RDWController` in the Government service requires an API key to be specified in the URL like this: `/rdw/{apiKey}/vehicle/{licenseNumber}`.
- The TrafficControl service reads this API key from a dapr secret store.

## Step 1: Add API key requirement to the RDW controller

1. Open the `Assignment 5` folder in this repo in VS Code.

First you going to change the `GetVehicleDetails` method of the `RDWController` in the Government service so it requires an API key.

2. Open the file `Assignment05/src/GovernmentService/Controllers/RDWController.cs` in VS Code.

3. Add a private constant field to the class to hold the API key:

   ```csharp
   private const string SUPER_SECRET_API_KEY = "A6k9D42L061Fx4Rm2K8";
   ```

4. Change the `GetVehicleDetails` method so it contains an API key part:

   ```csharp
    [HttpGet("rdw/{apikey}/vehicle/{licenseNumber}")]
    public ActionResult<VehicleInfo> GetVehicleDetails(string apiKey, string licenseNumber)
   ```

5. Add a check at the start of the method to check the API key:

   ```csharp
    if (apiKey != SUPER_SECRET_API_KEY)
    {
        return Unauthorized();
    }
   ```

   Obviously this is NOT the way you would implement security in a real-life system! But for now the focus is on the use of the dapr secret-store component and not the security of the sample application.

This concludes the work on the Government service.

## Step 2: Add a secret-store component

Before you can use the dapr secret-store from the TrafficControl service, we first have to add this component to the dapr configuration. By default, when you install dapr there are 3 components installed:

- pub/sub (Redis cache)
- State-store (Redis cache)
- Observability (Zipkin)

Each one of these components is configured using a yaml file in a well known location (e.g. on Windows this is the `.dapr/components` folder in your user's profile folder). By default, dapr uses these config files when starting an application with a dapr sidecar. But you can specify a different location on the command-line. You will do that later when you're testing the application and therefore you are going to create a custom components folder for the TrafficControl service.

1. Create a new folder: `Assignment05/src/TrafficControlService/components`.
2. Create a new file in this folder named `pubsub.yaml` and paste this snippet into the file:

   ```yaml
    apiVersion: dapr.io/v1alpha1
    kind: Component
    metadata:
    name: pubsub
    spec:
    type: pubsub.redis
    metadata:
    - name: redisHost
        value: localhost:6379
    - name: redisPassword
        value: ""
   ```

   This is how you configure dapr components. They have a name which you can use in your code to secify the component to use (remember the `pubsub` name you used in the previous assignment when publishing or subscribing to a pub/sub topic). They also have a type (to specify the building-block (pub/sub in this case) and component (Redis in this case)).

3. Create a new file in the components folder named `statestore.yaml` and paste this snippet into the file:

   ```yaml
    apiVersion: dapr.io/v1alpha1
    kind: Component
    metadata:
    name: statestore
    spec:
    type: state.redis
    metadata:
    - name: redisHost
        value: localhost:6379
    - name: redisPassword
        value: ""
    - name: actorStateStore
        value: "true"
   ```

4. Create a new file in the components folder named `zipkin.yaml` and paste this snippet into the file:

   ```yaml
    apiVersion: dapr.io/v1alpha1
    kind: Component
    metadata:
    name: zipkin
    spec:
    type: exporters.zipkin
    metadata:
    - name: enabled
        value: "true"
    - name: exporterAddress
        value: http://localhost:9411/api/v2/spans
   ```

   You basically recreated the default set of components as installed by dapr.

5. Create a new file in the components folder names `secrets.json` and paste this snippet into the file:

   ```json
    {
        "rdw-api-key": "A6k9D42L061Fx4Rm2K8"
    }
   ```

   This file holds the secrets you want to use in your application. Now we need a secret-store component that uses this file so we can read the secrets using the dapr client.

6. Create a new file in the components folder named `zipkin.yaml` and paste this snippet into the file:

   ```yaml
    apiVersion: dapr.io/v1alpha1
    kind: Component
    metadata:
    name: zipkin
    spec:
    type: exporters.zipkin
    metadata:
    - name: enabled
        value: "true"
    - name: exporterAddress
        value: http://localhost:9411/api/v2/spans
   ```
[WIP]