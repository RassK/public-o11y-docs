.. _dotnet-serverless-instrumentation:

*************************************************************************
Instrument your .NET AWS Lambda function for Splunk Observability Cloud
*************************************************************************

.. meta::
   :description: Follow these steps to instrument .NET lambda functions in AWS using OpenTelemetry to send traces to Splunk Observability Cloud.

You can instrument a .NET lambda function to send traces to Splunk Observability Cloud using the following OpenTelemetry template. The template uses these packages:

* :new-page:`OpenTelemetry <https://www.nuget.org/packages/OpenTelemetry0>`
* :new-page:`OpenTelemetry.Exporter.OpenTelemetryProtocol <https://www.nuget.org/packages/OpenTelemetry.Exporter.OpenTelemetryProtocol>` 
* :new-page:`OpenTelemetry.Instrumentation.AWS <https://www.nuget.org/packages/OpenTelemetry.Instrumentation.AWS>`
* :new-page:`OpenTelemetry.Instrumentation.AWSLambda <https://www.nuget.org/packages/OpenTelemetry.Instrumentation.AWSLambda>`
* :new-page:`OpenTelemetry.Instrumentation.Http <https://www.nuget.org/packages/OpenTelemetry.Instrumentation.Http>`
* :new-page:`OpenTelemetry.ResourceDetectors.AWS <https://www.nuget.org/packages/OpenTelemetry.ResourceDetectors.AWS>`

To instrument a .NET function in AWS Lambda for Splunk APM, follow these steps:

1. Integrate your existing lambda with the following template or start a new lambda using the template:

.. code-block:: csharp

   using Amazon.Lambda.Core;
   using OpenTelemetry;
   using OpenTelemetry.Exporter;
   using OpenTelemetry.Instrumentation.AWSLambda;
   using OpenTelemetry.ResourceDetectors.AWS;
   using OpenTelemetry.Resources;
   using OpenTelemetry.Trace;
   using System.Diagnostics;

   namespace DotNetInstrumentedLambdaExample;

   public class Function
   {
      public static readonly TracerProvider TracerProvider;

      static Function()
      {
         TracerProvider = ConfigureSplunkTelemetry()!;
      }

      // Note: Do not forget to point function handler to here.
      public string TracingFunctionHandler(string input, ILambdaContext context)
         => AWSLambdaWrapper.Trace(TracerProvider, FunctionHandler, input, context);

      public string FunctionHandler(string input, ILambdaContext context)
      {
         // TODO: Your function handler code here
      }

      private static TracerProvider ConfigureSplunkTelemetry()
      {
         var serviceName = Environment.GetEnvironmentVariable("AWS_LAMBDA_FUNCTION_NAME") ?? "Unknown";
         var accessToken = Environment.GetEnvironmentVariable("SPLUNK_ACCESS_TOKEN")?.Trim();
         var realm = Environment.GetEnvironmentVariable("SPLUNK_REALM")?.Trim();

         ArgumentNullException.ThrowIfNull(accessToken, "SPLUNK_ACCESS_TOKEN");
         ArgumentNullException.ThrowIfNull(realm, "SPLUNK_REALM");

         var builder = Sdk.CreateTracerProviderBuilder()
               // Use Add[instrumentation-name]Instrumentation to instrument missing services
               // Use Nuget to find different instrumentation libraries
               .AddHttpClientInstrumentation()
               .AddAWSInstrumentation()
               // Use AddSource to add your custom DiagnosticSource source names
               .AddSource(SourceName)
               .SetSampler(new AlwaysOnSampler())
               .AddAWSLambdaConfigurations(opts => opts.DisableAwsXRayContextExtraction = true)
               .SetResourceBuilder(
                  ResourceBuilder.CreateDefault()
                     .AddService(serviceName, serviceVersion: "1.0.0")
                     // Different resource detectors can be found at
                     // https://github.com/open-telemetry/opentelemetry-dotnet-contrib/tree/main/src/OpenTelemetry.ResourceDetectors.AWS#usage
                     .AddDetector(new AWSEBSResourceDetector()))
               .AddOtlpExporter(opts =>
               {
                  opts.Endpoint = new Uri($"https://ingest.{realm}.signalfx.com/v2/trace/otlp");
                  opts.Protocol = OtlpExportProtocol.HttpProtobuf;
                  opts.Headers = $"X-SF-TOKEN={accessToken}";
               });

         return builder.Build()!;
      }
   }

2. Make sure that the new function handler ``TracingFunctionHandler`` is configured as the main entry point by editing the aws-lambda-tools-defaults.json file and changing the ``function-handler`` entry. You can also do this using the AWS web console, changing the handler in :guilabel:`Runtime settings`.

3. The template expects the following environment variables:

   - ``AWS_LAMBDA_FUNCTION_NAME``: Name of your lambda function
   - ``SPLUNK_ACCESS_TOKEN``: Your Splunk ingest access token
   - ``SPLUNK_REALM``: Your Splunk ingest realm, for example ``us0``

4. The template also contains the following customization points in ``ConfigureSplunkTelemetry()``:

   - Add a custom instrumentation library to support other third-party libraries. You can search for libraries using NuGet and strings starting with ``OpenTelemetry.Instrumentation.``.
   - Some libraries already have ``System.Diagnostics.DiagnosticSource`` built in. Use the ``.AddSource()`` method to include a custom ``DiagnosticSource`` name.
   - The AWS package contains multiple ``ResourceDetectors`` that help describe your environment. Select a detector for your use case.

5. Add your code to the ``FunctionHandler`` function as the default AWS template expects.
