# dapr hands-on - Assignment 2 - Add dapr service invocation

In this assignment, you're going to add dapr into the mix. You will use the Service-to-service Invocation building-block. Before you start with this assignment, read the [introduction to this building-block](https://github.com/dapr/docs/blob/master/concepts/service-invocation/README.md) in the dapr documentation.

## Assignment goals

In order to complete this assignment, the following goals must be met:

- The Government service is started running dapr.
- The TrafficControl service uses the dapr client for .NET to call the GetVehicleInfo method on the Government service using a dapr direct service-to-service invocation.

## DIY instructions

First open the `Assignment 2` folder in this repo in VS Code. Then open the [dapr documentation](https://github.com/dapr/docs) and start hacking away. Make sure the Government service is using `50001` as the dapr-grpc-port. If you need any hints, you may peek in the step-by-step part.

## Step by step instructions

To get step-by-step instructions to achieve the goals, open the [step-by-step description](step-by-step.md).

## Next assignment

Go to [assignment 3](../Assignment03/README.md).
