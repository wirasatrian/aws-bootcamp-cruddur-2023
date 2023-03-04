# Week 2 — Observability

The definition of **observability** is a measure of how well the internal state of a system can be understood by examining its external output.
Observability addresses this challenge of understanding by providing tools and techniques for collecting and analyzing data from the system, such as logs, metrics, and traces which also known as 3 pillars of observability.  

But how will you generate outputs to measure a system? You will need to write additional code to expose its inner state and observe it. This process is called **Instrumentation**. Instrumentation is the code we write to measure different metrics like response times, etc.

**Telemetry** is the process of collecting and transmitting data from remote or inaccessible sources to monitoring and analysis. It encompasses instrumenting the system under observation and sending the data to a visualization tool. 

In this week :
  - I learned instrument backend flask application, collecting and transmitting telemetry data using Open Telemetry (OTEL), then explore and observe traces within [Honeycomb](honeycomb.io). I also create a custom span and add custom attribute on create message endpoint to record userid, message and time created then querying 
  - I learned instrument backend flask application, collecting anda transmitting telemetry data using AWS-Xray daemon, then explore and observe traces within AWS X-Ray Console
  - I write a custom logger to send application log data to CloudWatch Log group
  - I trying error logging and monitoring using [Rollbar](rollbar.com)


## Observability using Open Telemetry (OTEL) and Honeycomb

### Create Honeycomb Account

I follow [Gifted Lane Youtube Video](https://www.youtube.com/watch?v=7IwtVLfSD0o&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=10) to create [Honeycomb](honeycomb.io).
Login into honeycomb.io, then i create enviroment named `AWS Cloud Project Bootcamp', look up the API KEY. 

![Honeycomb Environment](assets/week2/create-env-honeycomb.png)

Execute this command to store and set the Honeycomb's API KEY on gitpod. Change `xxxxxxxxxxxx` with the API KEY.

```
export HONEYCOMB_API_KEY="xxxxxxxxxxxx"
export HONEYCOMB_SERVICE_NAME="Cruddur"
gp env HONEYCOMB_API_KEY="xxxxxxxxxxxx"
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

```
cd backend-flask
pip install -r requirements.txt
```

![Install Libraries](assets/week2/install-honeycomb-libraries.png)

Instrument backend flask application, by add codes below to the `app.py`

```
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

```
# Initialize automatic instrumentation with Flask
app = Flask(__name__)
FlaskInstrumentor().instrument_app(app)
RequestsInstrumentor().instrument()
```

![Instrument Backend App](assets/week2/instrument-backend-flask.png)


Add the following Environment Variables to `backend-flask` in 'docker-compose.yml' file

```
OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
OTEL_SERVICE_NAME: "${HONEYCOMB_SERVICE_NAME}"
```

![Environment Variables](assets/week2/add-env-vars-honeycomb.png)


### Run Application 

I right click `docker-compose.yml` file, then select **Compose Up**. I ensure all service are running and make the service port to be public.  

![Running Services](assets/week2/running-service-on-port-tab.png)


I go to Honeycomb.io datasets screen, and found the cruddur dataset were created successfully. 

![Cruddur dataset](assets/week2/hc-datasets-created.png)

I do some click and operation on frontend, go to look at honeycomb environment, and do query to list all of events recorded related with click on frontend.


![Query Events](assets/week2/hc-query-all-events.png)


Clik on the icon on left front one event row to view trace


![View Trace](assets/week2/hc-trace-on-home-activities-endpoint.png)


### Instrument Custom Span and Attribute

I tried to create a custom span and add custom attribute on **create message endpoint** to record userid, message, time created then querying, view tracing and visualizing.

![Custom Span](assets/week2/hc-instrument-custom-attr.png)

![Custom Span Events](assets/week2/hc-custom-span-userid-attr.png)

![Custom Span Trace](assets/week2/hc-custome-span-tracing.png)

![Visualizing](assets/week2/hc-visualizing.png)













