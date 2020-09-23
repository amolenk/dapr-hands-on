# dapr hands-on - Assignment 5 - Add secrets

In this assignment, you're going to add dapr secret management to retrieve an API key for calling the RDW Government service.

Before you start with this assignment, read the [introduction to this building-block](https://github.com/dapr/docs/blob/master/concepts/secrets/README.md) in the dapr documentation.

For this assignment you are supposed to use the file-based local secret-store component. This is only for development or testing purposes. Never use this component in production!

## Assignment goals

In order to complete this assignment, the following goals must be met:

- The `GetVehicleDetails` method of the `RDWController` in the Government service requires an API key to be specified in the URL like this: `/rdw/{apiKey}/vehicle/{licenseNumber}`.
- The TrafficControl service reads this API key from a dapr secret store.

## DIY instructions

First open the `Assignment 5` folder in this repo in VS Code. Then open the [dapr documentation](https://github.com/dapr/docs) and start hacking away. Make sure you use the default Redis pub/sub component provided out of the box by dapr.

## Next assignment

Make sure you stop all running processes before proceeding to the next assignment.

To get step-by-step instructions to achieve the goals, open the [step-by-step instructions](step-by-step.md).