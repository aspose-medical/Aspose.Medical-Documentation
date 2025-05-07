---
title: DICOM JSON Serialization/Deserialization
type: docs
weight: 10
url: /net/developer-guide/90-dicom-serialization/10-dicom-json-serialization/
---


# `DICOM` Serialization/Deserialization

`Aspose.Medical` provides robust and standards-compliant support for converting DICOM datasets to and from JSON format, adhering to the guidelines set forth by the DICOM standard (specifically outlined in PS3.18 - the DICOM Web Services standard). JSON-based representation of DICOM data is crucial for modern applications that interact with DICOM content over RESTful web services and in environments that prefer human-readable or easily parsable data formats such as JSON.

This document provides comprehensive guidance on how to use the `Aspose.Medical.Dicom.Serialization.DicomJsonSerializer`, `Aspose.Medical.Dicom.Serialization.DicomFileJsonConverter` and `Aspose.Medical.Dicom.Serialization.DatasetJsonConverter` classes to serialize and deserialize DICOM datasets and files to and from JSON format in .NET.

The `DicomJsonSerializer` class is designed for developers working with DICOM (Digital Imaging and Communications in Medicine) data, enabling smooth conversion to JSON for interoperability purposes such as web communication, auditing, debugging, or integration with JSON-based systems.

## Overview

`Aspose.Medical.Dicom.Serialization.DicomJsonSerializer` provides methods to:

- Serialize a `Dataset`, `DicomFile`, or an array of `Dataset` objects to `JSON`.
- Deserialize JSON back into `Dataset`, `DicomFile`, or an array of `Dataset` objects.
- Perform serialization from/to string or Stream.
- Customize serialization behavior using `DicomJsonSerializerOptions`.
- Format JSON output with optional pretty-print (indented) layout.

## Supported Types

The serializer supports converting the following objects to/from JSON:

- `Dataset`
- `DicomFile`
- `Dataset[]` (list of datasets)

## JSON Serialization

### Serialize Dataset to JSON string

```csharp
Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open("path/to/dicom/file.dcm");
Aspose.Medical.Dicom.Dataset dataset = dcm.Dataset;
string? json = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(dataset);
```

With pretty-print:

```csharp
Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open("path/to/dicom/file.dcm");
Aspose.Medical.Dicom.Dataset dataset = dcm.Dataset;
string? formattedJson = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(dataset, writeIndented: true);
```

### Serialize DicomFile to JSON string

```csharp
Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open("path/to/dicom/file.dcm");
string? json = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(dcm);
```

### Serialize List of Datasets to JSON string

```csharp
Aspose.Medical.Dicom.DicomFile dcm1 = DicomFile.Open("path/to/dicom/file-1.dcm");
Aspose.Medical.Dicom.DicomFile dcm2 = DicomFile.Open("path/to/dicom/file-2.dcm");
Aspose.Medical.Dicom.Dataset[] datasets = [dcm1.Dataset, dcm2.Dataset];
string? formattedJson = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(datasets, writeIndented: true);
```

### Serialize Dataset to JSON Stream (UTF-8)

```csharp
Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open("path/to/dicom/file.dcm");
Aspose.Medical.Dicom.Dataset dataset = dcm.Dataset;
using System.IO.MemoryStream stream = new();
Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(stream, dataset);
```

## JSON Deserialization

### Deserialize JSON string to Dataset

```csharp
Aspose.Medical.Dicom.Dataset? dataset = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Deserialize(jsonText);
```

### Deserialize JSON stream to Dataset

```csharp
using System.IO.FileStream stream = File.OpenRead("data.json");
Aspose.Medical.Dicom.Dataset? dataset = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Deserialize(stream);
```

### Deserialize JSON string to DicomFile

```csharp
Aspose.Medical.Dicom.DicomFile? dicomFile = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.DeserializeFile(jsonText);
```

### Deserialize JSON stream to DicomFile

```csharp
Aspose.Medical.Dicom.DicomFile? dicomFile = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.DeserializeFile(stream);
```

### Deserialize JSON string to list of Datasets

```csharp
Aspose.Medical.Dicom.Dataset[]? datasets = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.DeserializeList(jsonText);
```

### Deserialize JSON stream to list of Datasets

```csharp
Aspose.Medical.Dicom.Dataset[]? datasets = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.DeserializeList(stream);
```

## Serialization Options

All methods accept an optional `DicomJsonSerializerOptions` parameter. This allows customization of the serialization formats, such as tag presentation, VR usage, wrapping behavior, and others.

Example usage:

```csharp
DicomJsonSerializerOptions options = new()
{
    // Configure according to your needs
};

string json = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(dataset, options);
```

`Aspose.Medical.Dicom.Serialization.DicomJsonSerializerOptions` provides the following options:
- `UseKeywordsAsJsonKeys` - indicates whether to write the JSON keys as DICOM keywords instead of tags (ATTENTION: this is non-standard feature and may break parsers).
- `WriteKeyword` - specifies whether DICOM Tag keyword will be written as a distinct JSON attribute or not (ATTENTION: this is non-standard feature and may break parsers).
- `WriteName` - specifies whether DICOM Tag name will be written as a distinct JSON attribute or not (ATTENTION: this is non-standard feature and may break parsers).
- `BulkDataLoader` - a loader used to load blob data referenced in BulkData element.
- `BulkDataConverter` - a converter that allows writing DICOM Element data as a reference to a blob of data specified in the BulkData element.
- `NumberHandling` - (possible values are: `AsNumber`, `AsString`) determines how the DICOM numbers will be handled during serialization.

## Indentation (Pretty Printing)

JSON output can be formatted with indentation (for readability) using the `writeIndented` parameter:

```csharp
string prettyJson = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(dataset, writeIndented: true);
```

## Notes

- UTF-8 Stream methods are useful for low-level I/O and memory-efficient serialization, appropriate for web APIs or file interactions.
- `JsonNumberHandling.AllowNamedFloatingPointLiterals` allows parsing and serialization of special float values like NaN or Infinity.

## Example: Full Round-Trip

```csharp
// 1. Serialize a dataset to JSON string
string json = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(dataset, writeIndented: true);

// 2. Deserialize back to dataset
Aspose.Medical.Dicom.Dataset? restored = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Deserialize(json);
```

## Example: Serialize to and from File

```csharp
// Saving DICOM dataset as JSON
using System.IO.FileStream output = File.Create("output.json");
Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Serialize(output, dataset, writeIndented: true);

// Loading from file
using System.IO.FileStream input = File.OpenRead("output.json");
Aspose.Medical.Dicom.Dataset? loaded = Aspose.Medical.Dicom.Serialization.DicomJsonSerializer.Deserialize(input);
```

## Summary

| Operation                      | Input Type     | Output Type     | Method                |
|-------------------------------|----------------|------------------|------------------------|
| Serialize                     | Dataset         | string / stream | Serialize              |
| Serialize                     | DicomFile       | string / stream | Serialize              |
| Serialize                     | Dataset[]       | string / stream | Serialize              |
| Deserialize                   | string / stream | Dataset         | Deserialize            |
| DeserializeFile               | string / stream | DicomFile       | DeserializeFile        |
| DeserializeList               | string / stream | Dataset[]       | DeserializeList        |


## DicomFileJsonConverter, DatasetJsonConverter

`Aspose.Medical` provides `Aspose.Medical.Dicom.Serialization.DicomFileJsonConverter`, `Aspose.Medical.Dicom.Serialization.DatasetJsonConverter` for more control over serialization/deserialization. These classes could be added directly to `System.Text.Json.JsonSerializerOptions` and used with `System.Text.Json.JsonSerializer`:

```csharp
System.Text.Json.JsonSerializerOptions options = new();
options.Converters.Add(new DicomFileJsonConverter());
```
