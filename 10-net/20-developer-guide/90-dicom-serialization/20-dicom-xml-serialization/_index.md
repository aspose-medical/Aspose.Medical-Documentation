---
title: DICOM XML Serialization/Deserialization
type: docs
weight: 10
url: /net/developer-guide/90-dicom-serialization/20-dicom-xml-serialization/
---


# `XML` Serialization/Deserialization

`**Aspose**.Medical` provides robust and standards-compliant support for converting DICOM datasets to and from XML format, adhering to the guidelines set forth by the DICOM standard (specifically outlined in PS3.18 - the DICOM Web Services standard). XML-based representation of DICOM data is crucial for modern applications that interact with DICOM content over RESTful web services and in environments that prefer human-readable or easily parsable data formats such as XML.

This document provides comprehensive guidance on how to use the `Aspose.Medical.Dicom.Serialization.DicomXmlSerializer` class to serialize and deserialize DICOM datasets and files to and from XML format in .NET. The class is designed for developers working with DICOM (Digital Imaging and Communications in Medicine) data, enabling smooth conversion to XML for interoperability purposes such as web communication, auditing, debugging, or integration with XML-based systems.

## Overview


The `Aspose.Medical.Dicom.Serialization` namespace provides utilities to serialize and deserialize DICOM Dataset objects to and from XML format. DICOM (Digital Imaging and Communications in Medicine) is a standard for storing and transmitting medical imaging data. Converting DICOM data to an XML representation can make it easier to inspect, manipulate, or integrate with systems where XML processing is required.

This API enables both synchronous and asynchronous operations, supports streaming, and provides mechanisms to handle bulk binary data via plugins such as IBulkDataLoader and `IBulkDataConverter`.

### Available Methods

🔹 Synchronous Serialization

- `static string Serialize(Aspose.Medical.Dicom.Dataset dataset, DicomXmlSerializerOptions? options = null)`
- `static void Serialize(System.IO.Stream output, Aspose.Medical.Dicom.Dataset dataset, DicomXmlSerializerOptions? options = null)`

🔹 Asynchronous Serialization

- `static System.Threading.Tasks.Task<string> SerializeAsync(Aspose.Medical.Dicom.Dataset dataset, DicomXmlSerializerOptions? options = null)`
- `static System.Threading.Tasks.Task SerializeAsync(System.IO.Stream output, Aspose.Medical.Dicom.Dataset dataset, DicomXmlSerializerOptions? options = null)`

🔹 Synchronous Deserialization

- `static Aspose.Medical.Dicom.Dataset Deserialize(string xml, DicomXmlSerializerOptions? options = null)`
- `static Aspose.Medical.Dicom.Dataset Deserialize(System.IO.Stream stream, DicomXmlSerializerOptions? options = null)`

🔹 Asynchronous Deserialization

- `static System.Threading.Tasks.Task<Aspose.Medical.Dicom.Dataset> DeserializeAsync(string xml, DicomXmlSerializerOptions? options = null)`
- `static System.Threading.Tasks.Task<Aspose.Medical.Dicom.Dataset> DeserializeAsync(System.IO.Stream stream, DicomXmlSerializerOptions? options = null)`

## Options: Aspose.Medical.Dicom.Serialization.DicomXmlSerializerOptions

Represents configuration flags that control how serialization and deserialization behave.

| Property            | Type                                | Description                                                                                         |
|---------------------|-------------------------------------|-----------------------------------------------------------------------------------------------------|
| BulkDataLoader      | Aspose.Medical.Dicom.Serialization.IBulkDataLoader | Provides a mechanism for loading external binary bulk data indicated in the XML.                   |
| BulkDataConverter   | Aspose.Medical.Dicom.Serialization.IBulkDataConverter? | Converts DICOM elements into external binary chunks referenced in XML.                             |
| Default             | Aspose.Medical.Dicom.Serialization.DicomXmlSerializerOptions | Default options instance used if none are provided.                                                 |

```csharp
DicomXmlSerializerOptions options = DicomXmlSerializerOptions.Default;
```

# Serialization

Serialization is the process of converting a `Aspose.Medical.Dicom.Dataset` object into an XML string or stream.

---

## Synchronous Serialization

### Serialize to XML String

```csharp
// Create dataset
Aspose.Medical.Dicom.Dataset dataset = new();

// Populate dataset with elements
dataset.Add(new CodeString(Tag.DoseType, ["HEJ"]));

// Serialize
string xml = Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.Serialize(dataset);
```

With customized options:

```csharp
// Create dataset
Aspose.Medical.Dicom.Dataset dataset = new();

// Populate dataset with elements
dataset.Add(new CodeString(Tag.DoseType, ["HEJ"]));

// Serialize
Aspose.Medical.Dicom.Serialization.DicomXmlSerializerOptions options = new()
{
    // Replace with actual factory/impl
    BulkDataConverter = new CustomBulkDataConverter()
};

string xml = Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.Serialize(dataset, options);
```

### Serialize to Stream

```csharp
// Create dataset
Aspose.Medical.Dicom.Dataset dataset = new();

// Populate dataset with elements
dataset.Add(new CodeString(Tag.DoseType, ["HEJ"]));

// Serialize
using System.IO.MemoryStream stream = new();
Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.Serialize(stream, dataset);
// Use stream, e.g., save to file or network
```

## Asynchronous Serialization

### Serialize to XML String Asynchronously

```csharp
// Create dataset
Aspose.Medical.Dicom.Dataset dataset = new();

// Populate dataset with elements
dataset.Add(new CodeString(Tag.DoseType, ["HEJ"]));

// Serialize
string xml = await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.SerializeAsync(dataset);
```

With custom options:

```csharp
// Create dataset
Aspose.Medical.Dicom.Dataset dataset = new();

// Populate dataset with elements
dataset.Add(new CodeString(Tag.DoseType, ["HEJ"]));

// Serialize
Aspose.Medical.Dicom.Serialization.DicomXmlSerializerOptions options = new()
{
    // Replace with actual factory/impl
    BulkDataConverter = new CustomBulkDataConverter()
};

string xml = await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.SerializeAsync(dataset, options);
```

### Serialize to Stream Asynchronously

```csharp
// Create dataset
Aspose.Medical.Dicom.Dataset dataset = new();

// Populate dataset with elements
dataset.Add(new CodeString(Tag.DoseType, ["HEJ"]));

// Serialize
using System.IO.MemoryStream stream = new();
await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.SerializeAsync(stream, dataset);
// Use stream, e.g., save to file or network
```

# Deserialization

Deserialization is the process of converting an XML string or stream back into a `Aspose.Medical.Dicom.Dataset`.

## Synchronous Deserialization

### Deserialize from XML String

```csharp
string xml = "<DicomDataset>...</DicomDataset>"; // Replace with actual XML

// Deserialize
Aspose.Medical.Dicom.Dataset dataset = Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.Deserialize(xml);
```

With custom options:

```csharp
string xml = "<DicomDataset>...</DicomDataset>"; // Replace with actual XML

// Deserialize
Aspose.Medical.Dicom.Serialization.DicomXmlSerializerOptions options = new()
{
    BulkDataLoader = new CustomBulkDataLoader()
};

Aspose.Medical.Dicom.Dataset dataset = Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.Deserialize(xml, options);
```

### Deserialize from Stream

```csharp
using System.IO.Stream stream = new System.IO.FileStream("dicom.xml", FileMode.Open);
Aspose.Medical.Dicom.Dataset dataset = Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.Deserialize(stream);
```

## Asynchronous Deserialization

### Deserialize from XML String Asynchronously

```csharp
string xml = "<DicomDataset>...</DicomDataset>"; // Replace with actual XML
Aspose.Medical.Dicom.Dataset dataset = await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.DeserializeAsync(xml);
```

### Deserialize from Stream Asynchronously

```csharp
await using System.IO.Stream stream = new System.IO.FileStream("dicom.xml", FileMode.Open);
Aspose.Medical.Dicom.Dataset dataset = await Aspose.Medical.Dicom.Serialization.DicomXmlSerializer.DeserializeAsync(stream);
```

# Extending BulkData Handling

Implement custom bulk data strategies by extending the following interfaces:

- `Aspose.Medical.Dicom.Serialization.IBulkDataLoader`
- `Aspose.Medical.Dicom.Serialization.IBulkDataConverter`

These can be plugged into `DicomXmlSerializerOptions` to enable load/save large binary values through URI references or external files.

Example implementation:

```csharp
public class CustomBulkDataLoader : Aspose.Medical.Dicom.Serialization.IBulkDataLoader
{
    public byte[] Load(string uri)
    {
        // Logic to load bytes from URI
    }
}

public class CustomBulkDataConverter : Aspose.Medical.Dicom.Serialization.IBulkDataConverter
{
    public string Convert(byte[] data)
    {
        // Logic to convert data to external reference
    }
}
```

Then provide them in the serializer options:

```csharp
DicomXmlSerializerOptions options = new()
{
    BulkDataLoader = new CustomBulkDataLoader(),
    BulkDataConverter = new CustomBulkDataConverter()
};
```
