# Announcing Serverless Support for Socket.IO in Azure Web PubSub service

Announcing Serverless Support for Socket.IO in Azure Web PubSub Service
We are excited to announce the public preview of Socket.IO Serverless Mode in Azure Web PubSub service. This new mode eliminates the need for developers to maintain persistent connections on their application servers, offering a more streamlined and scalable approach. In addition to the existing default mode, developers can now deploy Socket.IO applications in a serverless environment using Azure Functions. This provides a stateless, highly scalable infrastructure, simplifying the development of real-time features while reducing both operational costs and maintenance overhead.

## What is Socket.IO Serverless Mode?

In the existing default mode, all Socket.IO clients connect directly to Azure Web PubSub. developers don't need to worry about if they have 100 or 1 million concurrent users. The service handles scaling up or down to meet the fluctuation of application users. However, the application server, which handles the business logic, must maintain a persistent connection with the service, adding complexity compared to stateless HTTP services.

- Persistent connections introduce challenges. Unlike HTTP services, which can quickly recover from downtime, servers with persistent connections require continuous uptime to manage client communication.
- Developer familiarity: Teams accustomed to stateless HTTP services may find persistent connections introduce engineering difficulties.
- Cost inefficiency: In scenarios with low-frequency real-time messaging, maintaining persistent connections results in unnecessary compute costs, such as managing "ping-pong" heartbeats used by Socket.IO for disconnection detection.

As serverless computing gains popularity, developers are seeking ways to reduce server management burdens while focusing on core business logic. Socket.IO Serverless Mode offers a flexible, serverless deployment model, allowing real-time, bi-directional communication between clients and servers without requiring persistent server connections.

In the new serverless mode, Socket.IO servers become stateless, pushing messages to clients via RESTful APIs and receiving messages via webhooks, all without compromising on real-time communication between clients and server. Socket.IO clients still have persistent connections with the service, but now developers can focus on writing backend logic as stateless Azure Functions, simplifying deployment and scaling.

This capability is not natively supported by [Socket.IO library](https://socket.io/) and is made possible by Azure Web PubSub for Socket.IO. It is part of our ongoing commitment to enhancing Socket.IO developers' experience and simplifying developing real-time applications.

## Differences Between Default Mode and Serverless Mode

Here is a common architecture of the default mode and serverless with using Azure Web PubSub for Socket.IO

![comparison diagram, side by side]()

| Feature | Default Mode | Serverless Mode |
|------------|------------|------------|
|Architecture|Persistent connections for both clients and servers.| Clients use persistent connections but servers use RESTful APIs and webhook event handlers in a stateless manner.|
| Azure Functions compatibility| No | Yes |
|SDKs and Languages| Requires the official JavaScript server SDKs together with [Extension library for Web PubSub for Socket.IO SDK](https://www.npmjs.com/package/@azure/web-pubsub-socket.io); All compatible clients|No mandatory SDKs or languages. Use [Socket.IO Function binding](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.WebPubSubForSocketIO) to simplify integrating with Azure Functions; All compatible clients|
|Network Accessibility| Servers do not need to expose network access as it proactively makes connection to the service|Servers must expose network access to the service for receiving webhook calls|
|Feature supports|Most features are supported, with some exceptions: [Unsupported server APIs of Socket.IO](./socketio-supported-server-apis.md)|Most common features are supported: [Supported functionality and RESTful APIs](./socket-io-serverless-protocol.md#supported-functionality-and-restful-apis)|

## Getting Started with Socket.IO Serverless Mode

Socket.IO Serverless Mode is ideal for scenarios that require lightweight, event-driven communication without the need for persistent backend connections. One of the best starting points is **broadcasting messages** to Socket.IO clients. Applications such as live sports scores, financial tickers, or real-time dashboards can benefit greatly from the scalability and cost effectiveness that serverless architecture offers.

To get started, follow our [Publish messages tutorial](https://learn.microsoft.com/azure/azure-web-pubsub/socket-io-serverless-tutorial-python) and build a real-time stock index application with Python and Azure Functions.

In addition to one-way broadcasting, **bi-directional** real-time communication between clients and servers remains a key feature of Socket.IO. Check out our [Build chat app tutorial](https://learn.microsoft.com/azure/azure-web-pubsub/socket-io-serverless-tutorial-javascript) for a step-by-step guide to implementing bi-directional communication using Socket.IO Serverless mode.

For a quick hands-on experience, visit our [QuickStart](https://learn.microsoft.com/azure/azure-web-pubsub/socket-io-serverless-quickstart) to see how to build and deploy a chat application with Socket.IO Serverless Mode, featuring identity-based authentication.
