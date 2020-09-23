# dapr hands-on - Assignment 5 - Add secrets

## Assignment goals

In order to complete this assignment, the following goals must be met:

- The `GetVehicleDetails` method of the `RDWController` in the Government service requires an API key to be specified in the URL like this: `/rdw/{apiKey}/vehicle/{licenseNumber}`.
- The TrafficControl service reads this API key from a dapr secret store.

## Step 1

1. Open the `Assignment 5` folder in this repo in VS Code.

...

