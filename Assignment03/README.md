# dapr hands-on - Assignment 3 - Add dapr state management

In this assignment, you're going to add dapr **state management** in the TrafficControl service to store vehicle information.

Before you start with this assignment, read the [introduction to this building-block](https://github.com/dapr/docs/blob/master/concepts/state-management/README.md) in the dapr documentation.

## Assignment goals

In order to complete this assignment, the following goals must be met:

- The TrafficControl service saves the state of a vehicle (VehicleState class) using the state management building block after vehicle entry.
- The TrafficControl service reads, updates and saves the state of a vehicle using the state management building block after vehicle exit.

For both these tasks you can use the dapr client for .NET.

## DIY instructions

First open the `Assignment 3` folder in this repo in VS Code. Then open the [dapr documentation](https://github.com/dapr/docs) and start hacking away. Make sure you use the default Redis state-store component provided out of the box by dapr.

## Step by step instructions

To get step-by-step instructions to achieve the goals, open the [step-by-step description](step-by-step.md).

## Next assignment

Go to [assignment 4](../Assignment04/README.md).
