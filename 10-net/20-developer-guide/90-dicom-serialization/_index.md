---
title: DICOM Serialization
type: docs
weight: 90
url: /net/developer-guide/90-dicom-serialization/
---


# DICOM Serialization and Deserialization

`Aspose.Medical` converts DICOM data to and from the DICOM JSON Model and Native DICOM Model XML. The serializers preserve DICOM tags, Value Representations (VRs), nested sequences, text encodings, numeric values, inline binary data, and external Bulk Data references.

The API offers several I/O shapes so an application can choose the one that fits its own data flow:

| Scenario | JSON | XML |
|---|---|---|
| Work with text already in memory | Serialize or deserialize a `string` | Serialize or deserialize a `string` |
| Read or write files and general .NET I/O | Synchronous or asynchronous `Stream` APIs | Synchronous or asynchronous `Stream` APIs |
| Integrate with ASP.NET Core, pipelines, or backpressure-aware components | `PipeReader` and `PipeWriter` APIs | `PipeReader` and `PipeWriter` APIs |
| Process a collection as items become available | Read or write `IAsyncEnumerable<Dataset?>` | Read consecutive XML fragments as `IAsyncEnumerable<Dataset>`; serialize one `Dataset` document at a time |
| Round-trip a complete DICOM file representation | `DicomFile` APIs | Not applicable; Native DICOM XML represents a dataset |
| Keep large values outside the document | `BulkDataURI`, `IBulkDataConverter`, and synchronous or asynchronous loaders | `BulkData`, `IBulkDataConverter`, and synchronous or asynchronous loaders |

Strings are convenient for short-lived in-memory transformations. Streams fit files, network responses, compression layers, and other established .NET components. Pipes allow serialization and deserialization to participate directly in producer/consumer flows while the application retains control of flushing, completion, cancellation, and buffer thresholds. JSON asynchronous enumeration is useful when a DICOM JSON array should be produced or consumed one dataset at a time.

Bulk Data handling is independent of the selected I/O shape. An `IBulkDataConverter` can write selected element values as external references. When deserializing XML or JSON that contains or may contain Bulk Data references, explicitly set `BulkDataLoader` in the serializer options. If the serializer encounters a Bulk Data reference without a configured loader, it throws `MedicalApiException`. `DefaultBulkDataLoader.Instance` is available for unauthenticated `file`, `http`, and `https` references, but it is not selected implicitly.

An `IBulkDataLoader` supplies referenced values synchronously. For asynchronous deserialization, prefer an `IAsyncBulkDataLoader` so the data source can use asynchronous I/O and observe the operation's cancellation token. `DefaultBulkDataLoader` implements `IAsyncBulkDataLoader`. These extension points let an application choose the storage policy, authentication model, buffering strategy, and concurrency appropriate for its environment.

## Summary

Use the simplest API that preserves the behavior your application needs:

- Choose strings for small, local transformations and debugging.
- Choose streams for files and general-purpose synchronous or asynchronous I/O.
- Choose `PipeReader` and `PipeWriter` for pipeline-based applications and explicit backpressure control.
- Choose JSON `IAsyncEnumerable<Dataset?>` APIs when dataset collections should be handled incrementally.
- Explicitly configure a Bulk Data loader before deserializing input that contains or may contain external references.
- Prefer `IAsyncBulkDataLoader` for asynchronous deserialization; use `DefaultBulkDataLoader.Instance` when its supported URI schemes and unauthenticated access fit the application.

See the format-specific pages for complete examples and ownership rules:

- [JSON Serialization/Deserialization]({{< ref "/10-net/20-developer-guide/90-dicom-serialization/10-dicom-json-serialization" >}} "JSON")
- [XML Serialization/Deserialization]({{< ref "/10-net/20-developer-guide/90-dicom-serialization/20-dicom-xml-serialization" >}} "XML")
