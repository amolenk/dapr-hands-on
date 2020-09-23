# dapr hands-on - Assignment 2 - Add dapr service invocation

## Assignment goals

In order to complete this assignment, the following goals must be met:

- The Government service is started running dapr.
- The TrafficControl service uses the dapr client for .NET to call the GetVehicleInfo method on the Government service using a dapr direct service-to-service invocation.

## Step 1: Run the Government service with dapr

### Step 1.1: Install dapr

If you haven't installed dapr stand-alone yet on your machine, first do that. If you already installed it, you can skip to step 1.2.

1. Make sure you have docker for desktop running on your machine.

2. Open a command-shell window.

3. enter the following command:

   ```
   dapr init
   ```

4. Check the logging for errors.

### Step 1.2: Start the government service with dapr

You started the government service in assignment 01 using `dotnet run`. When you want to run this service with a dapr side-car that handles its communication, you need to start it using the dapr CLI. There are a couple of things you need to specify when starting the service:

- The service needs a unique id which dapr can use to find it. You will use `governmentservice` as the id.
- The HTTP port the API is listening on is 6000. So you need to tell dapr that (so it can handle the communication).
- The default port used by dapr for gRPC communication if port 50001. You also need to tell dapr to use this port for gRPC communication.
- Finally you need to tell dapr how to start the service. This is again `dotnet run`.

You will use the `run` command of the dapr CLI and specify all the options above on the command-line:

1. Open a command-shell window and go to the `Assignment02/src/GovernmentService` folder in this repo.

2. enter the following command to run the service with a dapr sidecar:

   ```
   dapr run --app-id governmentservice --app-port 6000 --dapr-grpc-port 50001 dotnet run
   ```

3. Check the logs for any errors. As you can see, both dapr as well as application logging is shown as output.

That's it, you're now running the Government service with a dapr side-car. This means other services can use dapr to call this service. This is what you'll do in the next step.

## Step 2: Call the government service using service-to-service invocation

In this step, you're going to change the code of the TrafficControl service so it uses the dapr client for .NET to call the government service.

First you're going to add a reference to the dapr SDK for .NET:

1. Open the `Assignment 2` folder in this repo in VS Code.

2. Open the file `Assignment02/TrafficControlService/TrafficControlService.csproj` in VS Code.

3. Add a reference to the dapr SDK for .NET and the dapr ASP.NET Core integration version 0.10.0-preview01 by adding the following section to the file:

   ```xml
   <ItemGroup>
      <PackageReference Include="Dapr.Client" Version="0.10.0-preview01" />
      <PackageReference Include="Dapr.AspNetCore" Version="0.10.0-preview01" />
   </ItemGroup>
   ```

4. Open a command-shell window and go to the `Assignment02/src/TrafficControlService` folder in this repo.

5. Restore all references by executing `dotnet restore`.

Now you're going to use the dapr client to make the call to the Government service:

6. Open the file `Assignment02/src/TrafficControlService/Controllers/TrafficController.cs` in VS Code.

7. Add a using statements in the file to make sure you can use the dapr client:

   ```csharp
   using Dapr.Client;
   using Dapr.Client.Http;
   ```

8. Change the client injected into the `VehicleEntry` method from the IHttpClientFactory to the DaprClient:

   ```csharp
   public async Task<ActionResult> VehicleEntry(VehicleRegistered msg, [FromServices] DaprClient daprClient)

   ```

9. Change the part where the vehicle information is being retrieved to use the dapr client:

   ```csharp
   // get vehicle details
   var vehicleInfo = await daprClient.InvokeMethodAsync<VehicleInfo>(
      "governmentservice",
      $"rdw/vehicle/{msg.LicenseNumber}",
      new HTTPExtension { Verb = HTTPVerb.Get });
   ```

Now the dapr client is used to directly call a method on the Government service. Dapr will figure out where the service lives and handle the communication.

In order to make sure the dapr client is injected into the VehicleEntry method, you need to register is in the Startup class:

10. Open the file `Assignment02/src/TrafficControlService/Startup.cs` in VS Code.

11. Add a using statement in the file to make sure you can use the dapr client:

   ```csharp
   using Dapr.Client;
   using System.Text.Json;
   ```

12. Add code at the end of the `Configuration` method of the Startup class that registers the dapr client:

   ```csharp
   services.AddDaprClient(builder => builder.UseJsonSerializationOptions(new JsonSerializerOptions()
   {
         PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
         PropertyNameCaseInsensitive = true
   }));
   ```

   As you can see, a builder is specified to create the dapr client. The JsonSerializer options the dapr client must use for serializing and deserializing messages is passed in as an argument.

## Step 3: Test the application

Now you're going to start the TrafficControl service. This service does not need to run with a dapr sidecar because it uses the dapr client directly. Later you're going to add a side-car to this service.

13. Make sure the Government service is running with the dapr side-car (as you did in step 1.2).

14. Open a command-shell window and go to the `Assignment02/src/TrafficControlService` folder in this repo.

15. Check all your code-changes are correct by building the code:

   ```
   dotnet build
   ```

16. If you see any warnings or errors, review the previous steps to make sure the code is correct.

17. Start the TrafficControl service:

   ```
   dotnet run
   ```

The services are up & running. Now you're going to test this using the simulation.

18. Open a command-shell window and go to the `Assignment02/src/Simulation` folder in this repo.

19. Start the simulation:

   ```
   dotnet run
   ```

You should see similar logging as before when you ran the application.

## Step 4: Use dapr observability

So how can you check whether or not the call to the Government service is handled by dapr? Well, dapr has some observability built in. You can look at dapr traffic using Zipkin:

1. Open a browser and go the this url: [http://localhost:9411/zipkin](http://localhost:9411/zipkin).

2. Click the spyglass icon in the top right of the screen to search for traces.

3. You should see calls coming into the Government service. You can click any entry to get more details.

## Next assignment

Go to [assignment 3](../Assignment03/README.md).