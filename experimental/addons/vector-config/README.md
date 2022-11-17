# Vector Config

Vector is a lightweight, ultra-fast tool for building observability pipelines.

This addon provides a set of components/traits to simplify the usage of vector.

## Components

- vector-log2metric - A generic config model that support collect logs from files or pods, extract metrics with specified strategy, and send metrics to prometheus remote write endpoint.

## Demo

The demo example contains two resources:
- An Application CR that shows how to write a simple log2metric config with some fakedata
- A Pod that runs a Vector instance to receive config above, and print result of log2metric to its stdout

It is a fast && convenient way to explore how to do log2metric. 

First, apply demo resources:

`kubectl apply -f examples/vector-config/log2metric-demo.yaml`

You can observe the result of log2metric in Vector's console:

`kubectl logs -f --tail=10 vector-playground`

Feel free to edit the yaml, put in some log data that from your real applications, and try to generate some metrics. 