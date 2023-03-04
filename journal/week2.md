# Week 2 — Observability

The definition of **observability** is a measure of how well the internal state of a system can be understood by examining its external output.
Observability addresses this challenge of understanding by providing tools and techniques for collecting and analyzing data from the system, such as logs, metrics, and traces which also known as 3 pillars of observability.  

But how will you generate outputs to measure a system? You will need to write additional code to expose its inner state and observe it. This process is called **Instrumentation**. Instrumentation is the code we write to measure different metrics like response times, etc.

**Telemetry** is the process of collecting and transmitting data from remote or inaccessible sources to monitoring and analysis. It encompasses instrumenting the system under observation and sending the data to a visualization tool. 

In this week :
  - I learned instrument backend flask application, collecting and transmitting telemetry data using Open Telemetry (OTEL), then explore and observe traces within [Honeycomb](honeycomb.io)
  - I learned instrument backend flask application, collecting anda transmitting telemetry data using AWS-Xray daemon, then explore and observe traces within AWS X-Ray Console
  - I write a custom logger to send application log data to CloudWatch Log group
  - I trying error logging and monitoring using [Rollbar](rollbar.com)






