---
title: Developer Guide
type: docs
weight: 20
url: /net/developer-guide/save-dicom-file/
---


# Save `DICOM` file

A `DICOM` file contains medical imaging data used in healthcare applications. `Aspose.Medical` provides an API to save `DICOM` files while allowing the user to customize certain save options.

`Aspose.Medical` provides the `Aspose.Medical.Dicom.DicomFile.Save` method to save `DICOM` files. This method can save the file either to a disk (`Save(string fileName)`) or to a stream (`Save(Stream stream)`). Optionally, you can specify `Aspose.Medical.Dicom.Writers.SaveOptions` to customize how the file is saved.

## Saving `DICOM` to Files

To save a `DICOM` file to a specific location, use the `Aspose.Medical.Dicom.DicomFile.Save(System.string fileName, Aspose.Medical.Dicom.Writers.SaveOptions? options = null)` method. If no `SaveOptions` are provided, the default options will be used.

The following example demonstrates how to save a `DICOM` file to a specified path using default options:

```csharp
// Create an instance of DicomFile (assuming a DICOM file is already created or loaded)
Aspose.Medical.Dicom.DicomFile dicomFile = new();

// Define the output file path
string outputFilePath = "output.dcm";

// Save the DICOM file using default save options
dicomFile.Save(outputFilePath);
```

Save options in `Aspose.Medical.Dicom.Writers.SaveOptions` allow fine-grained control over how the `DICOM` file is saved. These options include:

* `KeepGroupLengths`: Defines whether to retain group length elements.
* `ExplicitLengthSequences`: Specifies if sequences should have an explicit length.
* `ExplicitLengthSequenceItems`: Specifies if sequence items should have an explicit length.

The following example demonstrates how to save a `DICOM` file to a specified path with custom options:

```csharp
// Create an instance of DicomFile (assuming a DICOM file is already created or loaded)
Aspose.Medical.Dicom.DicomFile dicomFile = new();

// Define the output file path
string outputFilePath = "output.dcm";

// Define custom save options
Aspose.Medical.Dicom.Writers.SaveOptions customOptions = new()
{
    KeepGroupLengths = false, // Remove group length elements
    ExplicitLengthSequences = true, // Use explicit length for sequences
    ExplicitLengthSequenceItems = true // Use explicit length for sequence items
};

// Save the DICOM file using default save options
dicomFile.Save(outputFilePath, customOptions);
```

## Saving `DICOM` to Streams

It is possible to save a `DICOM` file to a stream by passing an output stream to the `Aspose.Medical.Dicom.DicomFile` class `Save` method. There are many types of streams to which a `DICOM` file can be saved. In the below example, we have created a new `DICOM` file and Save the `DICOM` file to the stream.

```csharp
// Define custom save options
Aspose.Medical.Dicom.Writers.SaveOptions customOptions = new()
{
    KeepGroupLengths = false, // Remove group length elements
    ExplicitLengthSequences = true, // Use explicit length for sequences
    ExplicitLengthSequenceItems = true // Use explicit length for sequence items
};

using System.IO.MemoryStream memoryStream = new();
// Save the DICOM file using default save options
dicomFile.Save(memoryStream, customOptions);
```