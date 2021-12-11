# Distributed-tracing
Sample Distributed Tracing with Jaeger, this repo was created using the https://tracing.cloudnative101.dev/docs/lab-jaeger-java.html as sample.

In this repository we have two services created using programming language Java, Service A are responsible to call the Service B.

The libraries used are, opentracing-spring-cloud-starter, opentracing-contrib and jaeger-cient, the libraries used are below:
```
<dependency>
    <groupId>io.opentracing</groupId>
    <artifactId>opentracing-api</artifactId>
    <version>0.31.0</version>
</dependency>

<dependency>
    <groupId>io.opentracing.contrib</groupId>
    <artifactId>opentracing-spring-cloud-starter</artifactId>
    <version>0.1.13</version>
</dependency>

<dependency>
    <groupId>io.jaegertracing</groupId>
    <artifactId>jaeger-client</artifactId>
    <version>0.31.0</version>
</dependency>
```

## Service A
This service has a Bean and import the classes from jaegertracing.

```
import io.jaegertracing.Configuration;
import io.jaegertracing.Configuration.ReporterConfiguration;
import io.jaegertracing.Configuration.SamplerConfiguration;

@Bean
public io.opentracing.Tracer initTracer() {
  SamplerConfiguration samplerConfig = new SamplerConfiguration().withType("const").withParam(1);
  ReporterConfiguration reporterConfig = ReporterConfiguration.fromEnv().withLogSpans(true);
  return Configuration.fromEnv("service-a").withSampler(samplerConfig).withReporter(reporterConfig).getTracer();
}


```

## Service B

Same as service A, you must import the classes from jaegertracing and create a bean to initTracer.


## Create and Run the container using Podman
```
podman-compose build
podman-compose up
```

## Testing
Send request to the service
```
i=0;
while [ $i -lt 15 ];
do curl http://localhost:8080/sayHello/Carlos -I -s | head -n 1; i=$((i+1));
done;
```

## Open the Jaeger UI
```
open http://localhost:16686/jaeger
```

