---
title: DICOM JSON Serialization/Deserialization
type: docs
weight: 10
url: /net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/
---


# DICOM JSON Serialization and Deserialization

`Aspose.Medical.Dicom.Serialization.DicomJsonSerializer` converts DICOM datasets to and from UTF-8 DICOM JSON. It supports in-memory text, streams, pipelines, complete dataset arrays, and incremental asynchronous dataset sequences.

## Choose an API

| Data shape | Serialize | Deserialize |
|---|---|---|
| One `Dataset` | `Serialize` or `SerializeAsync` | `Deserialize` or `DeserializeAsync` |
| One `DicomFile` | `Serialize` or `SerializeAsync` | `DeserializeFile` or `DeserializeFileAsync` |
| A complete `Dataset[]` | `Serialize` or `SerializeAsync` | `DeserializeList` or `DeserializeListAsync` |
| An incremental dataset sequence | `SerializeAsync` with `IAsyncEnumerable<Dataset?>` | `DeserializeAsyncEnumerable` |

`IAsyncEnumerable<Dataset?>` is the incremental collection API. On serialization, datasets can arrive from an asynchronous producer while the JSON array is being written. On deserialization, `DeserializeAsyncEnumerable` exposes each array item to the application as it becomes available.

String overloads are synchronous. `Stream` overloads are available for synchronous and asynchronous I/O. `PipeReader` and `PipeWriter` overloads integrate with pipeline-based applications; they support cancellation and leave endpoint lifetime under caller control.

`DicomFile` JSON is an Aspose.Medical composite representation containing file meta information and the dataset. When an external service expects a DICOM JSON dataset object, serialize `dicomFile.Dataset`. Use the `DicomFile` overloads when both sides use the composite file representation and need to round-trip the meta information as well.

## Serialize and deserialize in memory

```csharp
var dicomFile = Aspose.Medical.Dicom.DicomFile.Open("input.dcm");
Aspose.Medical.Dicom.Dataset source = dicomFile.Dataset;

string json = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(
    source,
    writeIndented: true);
Aspose.Medical.Dicom.Dataset? restored =
    Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Deserialize(json);
```

The JSON root can be `null`, so JSON deserialization methods return nullable results.

Use the file-specific methods to preserve the Aspose.Medical file wrapper:

```csharp
string json = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(
    dicomFile,
    writeIndented: true);
Aspose.Medical.Dicom.DicomFile? restoredFile =
    Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.DeserializeFile(json);
```

For an in-memory collection:

```csharp
Aspose.Medical.Dicom.Dataset[] source = [firstDataset, secondDataset];

string json = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(source);
Aspose.Medical.Dicom.Dataset[]? restored =
    Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.DeserializeList(json);
```

## Use streams

Synchronous stream methods read or write UTF-8 JSON and leave the supplied stream open:

```csharp
using (System.IO.FileStream output = System.IO.File.Create("dataset.json"))
{
    Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(
        output,
        dataset,
        writeIndented: true);
}

using System.IO.FileStream input = System.IO.File.OpenRead("dataset.json");
Aspose.Medical.Dicom.Dataset? restored =
    Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Deserialize(input);
```

Use asynchronous stream methods when the surrounding workflow is asynchronous:

```csharp
await using (System.IO.FileStream output = System.IO.File.Create("dataset.json"))
{
    await Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.SerializeAsync(
        output,
        dataset,
        writeIndented: false);
}

await using System.IO.FileStream input = System.IO.File.OpenRead("dataset.json");
Aspose.Medical.Dicom.Dataset? restored =
    await Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.DeserializeAsync(input);
```

Equivalent stream overloads are available for `DicomFile` and `Dataset[]`.

## Use PipeReader and PipeWriter

Pipeline overloads can connect the serializer directly to ASP.NET Core request and response bodies, `System.IO.Pipelines.Pipe`, or another pipeline component. They consume and publish data incrementally and cooperate with pipe backpressure.

| JSON root | Read from `PipeReader` | Write to `PipeWriter` |
|---|---|---|
| `Dataset` | `DeserializeAsync` | `SerializeAsync` |
| `DicomFile` | `DeserializeFileAsync` | `SerializeAsync` |
| `Dataset[]` | `DeserializeListAsync` | `SerializeAsync` |
| Incremental dataset sequence | `DeserializeAsyncEnumerable` | `SerializeAsync` with `IAsyncEnumerable<Dataset?>` |

This example runs a producer and consumer over the same pipe:

```csharp
System.IO.Pipelines.Pipe pipe = new();

try
{
    System.Threading.Tasks.Task<Aspose.Medical.Dicom.Dataset?> readTask =
        Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.DeserializeAsync(
            pipe.Reader,
            cancellationToken: cancellationToken).AsTask();

    await Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.SerializeAsync(
        pipe.Writer,
        dataset,
        writeIndented: false,
        cancellationToken: cancellationToken);

    Aspose.Medical.Dicom.Dataset? restored = await readTask;
}
finally
{
    await pipe.Writer.CompleteAsync();
    await pipe.Reader.CompleteAsync();
}
```

The asynchronous `PipeWriter` overloads flush serialized bytes but do not complete the writer. Deserialization does not complete the `PipeReader`.

A JSON `PipeReader` deserializer consumes one complete JSON root and leaves following bytes unread. This makes it possible for an application protocol to frame another payload after the JSON value. The caller still owns that framing and the pipe endpoints.

In ASP.NET Core, the same API shape can be used directly with `HttpRequest.BodyReader` and `HttpResponse.BodyWriter`:

```csharp
Aspose.Medical.Dicom.Dataset? requestDataset =
    await Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.DeserializeAsync(
        httpContext.Request.BodyReader,
        cancellationToken: httpContext.RequestAborted);

httpContext.Response.ContentType = "application/dicom+json";
await Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.SerializeAsync(
    httpContext.Response.BodyWriter,
    responseDataset,
    cancellationToken: httpContext.RequestAborted);
```

The web host owns these pipe endpoints; application code should not complete them.

## Process dataset collections incrementally with IAsyncEnumerable

`SerializeAsync` accepts `IAsyncEnumerable<Dataset?>` and writes one JSON array while the producer supplies datasets. The serializer does not require the producer to create a `Dataset[]` first. For example, a channel can feed datasets to the serializer as another component produces them:

```csharp
static async System.Collections.Generic.IAsyncEnumerable<Aspose.Medical.Dicom.Dataset?>
    ReadDatasetsAsync(
        System.Threading.Channels.ChannelReader<Aspose.Medical.Dicom.Dataset> source,
        [System.Runtime.CompilerServices.EnumeratorCancellation]
        System.Threading.CancellationToken cancellationToken = default)
{
    await foreach (Aspose.Medical.Dicom.Dataset dataset in
                   source.ReadAllAsync(cancellationToken))
        yield return dataset;
}

static async System.Threading.Tasks.Task SerializeDatasetsAsync(
    System.IO.Stream output,
    System.Threading.Channels.ChannelReader<Aspose.Medical.Dicom.Dataset> source,
    System.Threading.CancellationToken cancellationToken)
{
    await Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.SerializeAsync(
        output,
        ReadDatasetsAsync(source, cancellationToken),
        writeIndented: false,
        cancellationToken: cancellationToken);
}
```

The `PipeWriter` overload provides the same incremental sequence support for pipeline destinations.

To consume a JSON array one dataset at a time, use `DeserializeAsyncEnumerable`:

```csharp
await using System.IO.FileStream input = System.IO.File.OpenRead("studies.json");

await foreach (Aspose.Medical.Dicom.Dataset? dataset in
               Aspose.Medical.Dicom.Serialization.DicomJsonSerializer
                   .DeserializeAsyncEnumerable(
                       input,
                       cancellationToken: cancellationToken))
{
    if (dataset is null)
        continue;

    await ProcessDatasetAsync(dataset, cancellationToken);
}
```

Each dataset is yielded when it is ready for use. A `null` array item is yielded as `null`; a `null` JSON root produces an empty sequence. The stream remains open, and any content following the array must be whitespace. A `PipeReader` overload is also available and leaves the caller responsible for completing the reader.

Use `DeserializeListAsync` instead when the application needs the complete `Dataset[]` result before continuing.

The reader and writer overloads can be connected directly. This example relays a DICOM JSON array from one pipe to another without first creating a `Dataset[]`:

```csharp
static async System.Threading.Tasks.Task RelayDatasetsAsync(
    System.IO.Pipelines.PipeReader source,
    System.IO.Pipelines.PipeWriter destination,
    System.Threading.CancellationToken cancellationToken)
{
    System.Collections.Generic.IAsyncEnumerable<Aspose.Medical.Dicom.Dataset?> datasets =
        Aspose.Medical.Dicom.Serialization.DicomJsonSerializer
            .DeserializeAsyncEnumerable(
            source,
            cancellationToken: cancellationToken);

    await Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.SerializeAsync(
        destination,
        datasets,
        writeIndented: false,
        cancellationToken: cancellationToken);
}
```

`RelayDatasetsAsync` leaves both endpoints open. Their owner remains responsible for completion and for deciding how failures are propagated through the surrounding pipeline.

## Load BulkDataURI references

DICOM JSON can carry an element value inline or refer to it through `BulkDataURI`. If the input contains or may contain `BulkDataURI`, explicitly set `BulkDataLoader` in `DicomJsonSerializerOptions`. If the serializer encounters `BulkDataURI` without a configured loader, it throws `Aspose.Medical.Errors.MedicalApiException`.

`DefaultBulkDataLoader` supports unauthenticated `file`, `http`, and `https` references. It is not selected automatically; opt in explicitly when that behavior fits the application:

```csharp
Aspose.Medical.Dicom.Serialization.DicomJsonSerializerOptions options = new()
{
    BulkDataLoader =
        Aspose.Medical.Dicom.Serialization.DefaultBulkDataLoader.Instance
};

Aspose.Medical.Dicom.Dataset? dataset =
    await Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.DeserializeAsync(
        jsonStream,
        options,
        cancellationToken);
```

Configure a custom loader when the source requires authentication, authorization, URI mapping, caching, object-store access, or another application-specific policy.

`IBulkDataLoader` defines synchronous loading. For `DeserializeAsync`, `DeserializeFileAsync`, `DeserializeListAsync`, and `DeserializeAsyncEnumerable`, prefer a loader that implements `IAsyncBulkDataLoader`. Its `GetDataAsync` method lets the data source use asynchronous I/O and observe the deserialization cancellation token. A synchronous-only `IBulkDataLoader` can still be supplied to asynchronous methods, but it does not make the underlying retrieval asynchronous. The returned memory becomes available to the DICOM element, so its backing storage must remain valid for as long as the element can access it.

This file-based loader supports both synchronous and asynchronous calls:

```csharp
sealed class FileBulkDataLoader :
    Aspose.Medical.Dicom.Serialization.IAsyncBulkDataLoader
{
    public System.Span<byte> GetData(string uri)
        => System.IO.File.ReadAllBytes(GetPath(uri));

    public async System.Threading.Tasks.ValueTask<System.Memory<byte>> GetDataAsync(
        string uri,
        System.Threading.CancellationToken cancellationToken)
    {
        byte[] data = await System.IO.File.ReadAllBytesAsync(
            GetPath(uri),
            cancellationToken).ConfigureAwait(false);

        return data;
    }

    private static string GetPath(string uri) => new System.Uri(uri).LocalPath;
}
```

Provide it through `DicomJsonSerializerOptions`:

```csharp
Aspose.Medical.Dicom.Serialization.DicomJsonSerializerOptions options = new()
{
    BulkDataLoader = new FileBulkDataLoader()
};

Aspose.Medical.Dicom.Dataset? dataset =
    await Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.DeserializeAsync(
        jsonPipeReader,
        options,
        cancellationToken);
```

Because the loader is selected through the serializer options, the same JSON code can be integrated with an authenticated HTTP client, cloud storage SDK, database, cache, or another asynchronous data source chosen by the application.

## Write BulkDataURI references

Set `BulkDataConverter` to choose which supported element values should be represented by a URI. Return `null` to keep a value inline:

```csharp
sealed class PixelDataUriConverter(string pixelDataUri) :
    Aspose.Medical.Dicom.Serialization.IBulkDataConverter
{
    public string? GetBulkDataUri(Aspose.Medical.Dicom.Elements.IElement element)
        => element.Tag == Aspose.Medical.Dicom.Tags.Tag.PixelData
            ? pixelDataUri
            : null;
}

Aspose.Medical.Dicom.Serialization.DicomJsonSerializerOptions options = new()
{
    BulkDataConverter = new PixelDataUriConverter(
        "https://storage.example/studies/1/pixel-data")
};

string json = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(
    dataset,
    options);
```

The converter supplies the reference written into JSON; it does not store the element bytes. The application must make the referenced content available through its storage workflow before publishing the JSON.

## Configure JSON output

`DicomJsonSerializerOptions` provides these controls:

| Property | Purpose |
|---|---|
| `BulkDataLoader` | Supplies values for `BulkDataURI` references. It must be set explicitly when the input contains or may contain those references. |
| `BulkDataConverter` | Selects values to represent as `BulkDataURI` during serialization. |
| `NumberHandling` | Writes `IS`, `DS`, `SV`, and `UV` values as JSON numbers (`AsNumber`) or strings (`AsString`). |
| `UseKeywordsAsJsonKeys` | Uses DICOM keywords instead of tag numbers as property names. This is non-standard DICOM JSON. |
| `WriteKeyword` | Adds a keyword attribute. This is non-standard DICOM JSON. |
| `WriteName` | Adds a tag-name attribute. This is non-standard DICOM JSON. |

For example:

```csharp
Aspose.Medical.Dicom.Serialization.DicomJsonSerializerOptions options = new()
{
    NumberHandling =
        Aspose.Medical.Dicom.Serialization.DicomJsonNumberHandling.AsString
};

string json = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(
    dataset,
    options,
    writeIndented: true);
```

Keep the keyword and name extensions disabled when interoperating with consumers that require the standard DICOM JSON Model.

## Summary

`DicomJsonSerializer` covers small in-memory transformations, files and generic streams, ASP.NET Core and pipeline integration, complete DICOM dataset collections, and incremental asynchronous collection processing. Bulk Data loaders and converters keep URI resolution and external storage policy under application control. Select the overload that matches the surrounding I/O architecture so the application can control cancellation, backpressure, allocation boundaries, and object lifetime without adding adapter models.
