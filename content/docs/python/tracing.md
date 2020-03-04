---
title: "Tracing"
---

This page contains documentation for OpenTelemetry Python.

# Quick Start

**Please note** that this library is currently in *alpha*, and shouldn't be used in production environments.

The API and SDK packages are available on PyPI, and can installed via `pip`:

```bash
pip install opentelemetry-api
pip install opentelemetry-sdk
```

From there, you should be able to use opentelemetry as per the following:

```python
from opentelemetry import trace
from opentelemetry.context import Context
from opentelemetry.sdk.trace import TracerSource
from opentelemetry.sdk.trace.export import ConsoleSpanExporter
from opentelemetry.sdk.trace.export import SimpleExportSpanProcessor

trace.set_preferred_tracer_source_implementation(lambda T: TracerSource())
tracer = trace.get_tracer(__name__)
trace.tracer_source().add_span_processor(
    SimpleExportSpanProcessor(ConsoleSpanExporter())
)
with tracer.start_as_current_span("foo"):
    with tracer.start_as_current_span("bar"):
        with tracer.start_as_current_span("baz"):
            print(Context)
```

Running the code will output trace information to the console:

```bash
<class 'opentelemetry.context.context.Context'>
Span(name="baz", context=SpanContext(trace_id=0x1c2fa8c9f31f2b933a3a927d5ce99eca, span_id=0x2045d5b31fe7f7c5, trace_state={}), kind=SpanKind.INTERNAL, parent=Span(name="bar", context=SpanContext(trace_id=0x1c2fa8c9f31f2b933a3a927d5ce99eca, span_id=0xdbbfc1bfca8634f5, trace_state={})), start_time=2020-03-04T07:51:53.547822Z, end_time=2020-03-04T07:51:53.547848Z)
Span(name="bar", context=SpanContext(trace_id=0x1c2fa8c9f31f2b933a3a927d5ce99eca, span_id=0xdbbfc1bfca8634f5, trace_state={}), kind=SpanKind.INTERNAL, parent=Span(name="foo", context=SpanContext(trace_id=0x1c2fa8c9f31f2b933a3a927d5ce99eca, span_id=0x5582cf9abdc107ae, trace_state={})), start_time=2020-03-04T07:51:53.547801Z, end_time=2020-03-04T07:51:53.547940Z)
Span(name="foo", context=SpanContext(trace_id=0x1c2fa8c9f31f2b933a3a927d5ce99eca, span_id=0x5582cf9abdc107ae, trace_state={}), kind=SpanKind.INTERNAL, parent=None, start_time=2020-03-04T07:51:53.547773Z, end_time=2020-03-04T07:51:53.548005Z)

```

# API Reference

See the [API documentation](https://open-telemetry.github.io/opentelemetry-python/) for more detail, and the [opentelemetry-example-app](https://github.com/open-telemetry/opentelemetry-python/blob/master/examples/opentelemetry-example-app/README.rst) for a complete example.
