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


## Observability using Open Telemetry (OTEL) and Honeycomb

### Create Honeycomb Account

I follow [Gifted Lane Youtube Video](https://www.youtube.com/watch?v=7IwtVLfSD0o&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=10) to create [Honeycomb](honeycomb.io), look up the API KEY, then store and set the Honeycomb's API KEY on gitpod enviroment.

```
export HONEYCOMB_API_KEY=""
export HONEYCOMB_SERVICE_NAME="Cruddur"
gp env HONEYCOMB_API_KEY=""
gp env HONEYCOMB_SERVICE_NAME="Cruddur"
```

### Install Open Telemetry Python Libray

I add the following library to `requirements.txt` file:

```
opentelemetry-api 
opentelemetry-sdk 
opentelemetry-exporter-otlp-proto-http 
opentelemetry-instrumentation-flask 
opentelemetry-instrumentation-requests
```

Then I install those libraries and dependencies:

```sh
pip install -r requirements.txt
```

Instrument backend flask application, by add codes below to the `app.py`

```py
from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor

# Initialize tracing and an exporter that can send data to Honeycomb
provider = TracerProvider()
processor = BatchSpanProcessor(OTLPSpanExporter())
provider.add_span_processor(processor)
trace.set_tracer_provider(provider)
tracer = trace.get_tracer(__name__)
```

```py
# Initialize automatic instrumentation with Flask
app = Flask(__name__)
FlaskInstrumentor().instrument_app(app)
RequestsInstrumentor().instrument()

Add the following Environment Variables to `backend-flask` in 'docker-compose.yml' file

```yml
OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
OTEL_SERVICE_NAME: "${HONEYCOMB_SERVICE_NAME}"
```








