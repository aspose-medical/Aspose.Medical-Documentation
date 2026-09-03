---
title: DICOM XML Serialization/Deserialization
type: docs
weight: 10
url: /net/developer-guide/90-dicom-serialization/20-dicom-xml-serialization/
---


# DICOM XML Serialization and Deserialization

`Aspose.Medical.Dicom.Serialization.DicomXmlSerializer` converts a DICOM `Dataset` to and from Native DICOM Model XML. The serializer supports synchronous and asynchronous text and stream workflows, caller-owned `PipeReader` and `PipeWriter` endpoints, incremental reading of consecutive XML fragments, cancellation-aware asynchronous operations, inline binary values, and external Bulk Data references.

## Choose an API

| Scenario | Serialize | Deserialize |
|---|---|---|
| XML text in memory | `Serialize(Dataset, ...)` | `Deserialize(string, ...)` |
| General .NET I/O | `Serialize(Stream, ...)` or `SerializeAsync(Stream, ...)` | `Deserialize(Stream, ...)` or `DeserializeAsync(Stream, ...)` |
| Pipeline-based I/O | `SerializeAsync(PipeWriter, ...)` | `DeserializeAsync(PipeReader, ...)` |
| Consecutive XML fragments | Serialize one `Dataset` document at a time | `DeserializeAsyncEnumerable(Stream, ...)` or `DeserializeAsyncEnumerable(PipeReader, ...)` |

Native DICOM Model XML represents one dataset per document. Use the single-result deserialization methods for one document and `DeserializeAsyncEnumerable` to read consecutive Native DICOM Model XML fragments.

## Serialize and deserialize in memory

```csharp
var dicomFile = Aspose.Medical.Dicom.DicomFile.Open("input.dcm");
Aspose.Medical.Dicom.Dataset source = dicomFile.Dataset;

string xml = Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.Serialize(source);
Aspose.Medical.Dicom.Dataset restored =
    Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.Deserialize(xml);
```

The asynchronous text overloads accept a cancellation token:

```csharp
string xml = await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.SerializeAsync(
    dataset,
    cancellationToken: cancellationToken);

Aspose.Medical.Dicom.Dataset restored =
    await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.DeserializeAsync(
        xml,
        cancellationToken: cancellationToken);
```

## Use streams

Synchronous stream methods leave the supplied stream open:

```csharp
using (System.IO.FileStream output = System.IO.File.Create("dataset.xml"))
{
    Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.Serialize(
        output,
        dataset);
}

using System.IO.FileStream input = System.IO.File.OpenRead("dataset.xml");
Aspose.Medical.Dicom.Dataset restored =
    Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.Deserialize(input);
```

Asynchronous stream methods perform asynchronous I/O and propagate cancellation:

```csharp
await using (System.IO.FileStream output = System.IO.File.Create("dataset.xml"))
{
    await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.SerializeAsync(
        output,
        dataset,
        cancellationToken: cancellationToken);
}

await using System.IO.FileStream input = System.IO.File.OpenRead("dataset.xml");
Aspose.Medical.Dicom.Dataset restored =
    await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.DeserializeAsync(
        input,
        cancellationToken: cancellationToken);
```

Streams remain owned by the caller, which makes these overloads suitable for file streams, network streams, compression layers, and other composed .NET I/O.

## Use PipeReader and PipeWriter

Pipeline overloads allow Native DICOM XML to flow directly through `System.IO.Pipelines`, including ASP.NET Core request and response bodies. Serialization publishes XML incrementally and cooperates with pipe backpressure; deserialization consumes data as it becomes available.

```csharp
System.IO.Pipelines.Pipe pipe = new();

try
{
    System.Threading.Tasks.Task<Aspose.Medical.Dicom.Dataset> readTask =
        Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.DeserializeAsync(
            pipe.Reader,
            cancellationToken: cancellationToken);

    await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.SerializeAsync(
        pipe.Writer,
        dataset,
        cancellationToken: cancellationToken);

    Aspose.Medical.Dicom.Dataset restored = await readTask;
}
finally
{
    await pipe.Writer.CompleteAsync();
    await pipe.Reader.CompleteAsync();
}
```

The serializer flushes the XML document but does not complete the `PipeWriter`. The deserializer does not complete the `PipeReader`. The caller remains responsible for both endpoints.

An XML reader may buffer beyond the document element. Supply one complete XML document to each `DeserializeAsync(PipeReader, ...)` invocation. Use `DeserializeAsyncEnumerable` when a stream or pipe contains consecutive Native DICOM Model XML fragments.

In ASP.NET Core, use the request and response pipelines directly:

```csharp
Aspose.Medical.Dicom.Dataset requestDataset =
    await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.DeserializeAsync(
        httpContext.Request.BodyReader,
        cancellationToken: httpContext.RequestAborted);

httpContext.Response.ContentType = "application/dicom+xml";
await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.SerializeAsync(
    httpContext.Response.BodyWriter,
    responseDataset,
    cancellationToken: httpContext.RequestAborted);
```

The web host owns these endpoints; application code should not complete them.

## Read consecutive XML fragments

`DeserializeAsyncEnumerable` reads one dataset for each Native DICOM Model XML fragment and leaves the stream open:

```csharp
await using System.IO.FileStream input = System.IO.File.OpenRead("datasets.xml");

await foreach (Aspose.Medical.Dicom.Dataset dataset in
               Aspose.Medical.Dicom.Serialization.DicomXmlSerializer
                   .DeserializeAsyncEnumerable(
                       input,
                       cancellationToken: cancellationToken))
{
    await ProcessDatasetAsync(dataset, cancellationToken);
}
```

The `PipeReader` overload provides the same fragment-sequence behavior and leaves the caller responsible for completing the reader.

## Load Bulk Data

Native DICOM XML can represent an element value with a `<BulkData>` reference using a URI or UUID. If the input contains or may contain `<BulkData>` elements, explicitly set `BulkDataLoader` in `DicomXmlSerializerOptions`. If the serializer encounters a `<BulkData>` element without a configured loader, it throws `Aspose.Medical.Errors.MedicalApiException`.

`DefaultBulkDataLoader` supports unauthenticated `file`, `http`, and `https` URI references. It is not selected automatically; opt in explicitly when that behavior fits the application:

```csharp
Aspose.Medical.Dicom.Serialization.DicomXmlSerializerOptions options = new()
{
    BulkDataLoader =
        Aspose.Medical.Dicom.Serialization.DefaultBulkDataLoader.Instance
};

Aspose.Medical.Dicom.Dataset dataset =
    await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.DeserializeAsync(
        xmlStream,
        options,
        cancellationToken);
```

Supply a custom loader for authenticated endpoints, UUID mapping, object storage, caching, databases, or another application-specific source.

`IBulkDataLoader` defines synchronous loading. For `DeserializeAsync` and `DeserializeAsyncEnumerable`, prefer a loader that implements `IAsyncBulkDataLoader`. Its `GetDataAsync` method lets the data source use asynchronous I/O and observe the deserialization cancellation token. A synchronous-only `IBulkDataLoader` can still be supplied to asynchronous methods, but it does not make the underlying retrieval asynchronous.

This loader resolves file URI references in either mode:

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

Aspose.Medical.Dicom.Serialization.DicomXmlSerializerOptions options = new()
{
    BulkDataLoader = new FileBulkDataLoader()
};

Aspose.Medical.Dicom.Dataset dataset =
    await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.DeserializeAsync(
        xmlStream,
        options,
        cancellationToken);
```

Because the loader is selected through the serializer options, the same XML code can be integrated with different storage policies without changing the DICOM parsing workflow.

## Write Bulk Data references

Set `BulkDataConverter` to choose supported values that should be represented externally. Returning `null` keeps an element value inline:

```csharp
sealed class PixelDataUriConverter(string pixelDataUri) :
    Aspose.Medical.Dicom.Serialization.IBulkDataConverter
{
    public string? GetBulkDataUri(Aspose.Medical.Dicom.Elements.IElement element)
        => element.Tag == Aspose.Medical.Dicom.Tags.Tag.PixelData
            ? pixelDataUri
            : null;
}

Aspose.Medical.Dicom.Serialization.DicomXmlSerializerOptions options = new()
{
    BulkDataConverter = new PixelDataUriConverter(
        "https://storage.example/studies/1/pixel-data")
};

string xml = Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.Serialize(
    dataset,
    options);
```

The resulting XML contains a `<BulkData uri="..." />` reference for Pixel Data. The converter supplies the reference only; the application remains responsible for storing the bytes and making them available to the corresponding loader or external consumer.

## Configure XML serialization

`DicomXmlSerializerOptions` contains:

| Property | Purpose |
|---|---|
| `BulkDataLoader` | Supplies values for URI or UUID Bulk Data references. It must be set explicitly when the input contains or may contain those references. |
| `BulkDataConverter` | Selects element values to represent as Bulk Data references during serialization. |
| `Default` | Provides the default option set. |

The same options apply to string, stream, and pipeline overloads.

## Summary

`DicomXmlSerializer` supports in-memory XML, synchronous and asynchronous streams, and backpressure-aware `PipeReader`/`PipeWriter` integration for one Native DICOM XML dataset document. Cancellation-aware asynchronous methods fit request processing, network I/O, and storage workflows. Bulk Data loaders and converters let the application choose how external values are stored, authorized, fetched, cached, and buffered without changing the dataset-facing serialization code.
