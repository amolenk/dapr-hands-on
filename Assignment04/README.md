# dapr hands-on - Assignment 4 - Add pub/sub messsaging

In this assignment, you're going to add dapr **publish/subscribe** messaging to send messages from the simulation to the TrafficControl service and from the TrafficControl service to the Government service.

Before you start with this assignment, read the [introduction to this building-block](https://github.com/dapr/docs/blob/master/concepts/publish-subscribe-messaging/README.md) in the dapr documentation.

## Assignment goals

In order to complete this assignment, the following goals must be met:

1. The simulation uses the dapr client to send messages over pub/sub to the TrafficControl service (vehicle entry and vehicle exit).
2. The TrafficControl service uses the dapr client to send messages to the Government service (for communicating speeding violations to the CJIB).

## DIY instructions

First open the `Assignment 4` folder in this repo in VS Code. Then open the [dapr documentation](https://github.com/dapr/docs) and start hacking away. Make sure you use the default Redis pub/sub component provided out of the box by dapr.

## Step by step instructions

To get step-by-step instructions to achieve the goals, open the [step-by-step instructions](step-by-step.md).

## Next assignment

Make sure you stop all running processes before proceeding to the next assignment.

Go to [assignment 5](../Assignment05/README.md).
