---
title: Save DICOM file
type: docs
weight: 30
url: /net/developer-guide/save-dicom-file/
---


# Save `DICOM` file

A `DICOM` file contains medical imaging data used in healthcare applications. `Aspose.Medical` provides an API to save `DICOM` files while allowing the user to customize certain save options.

`Aspose.Medical` exposes the following APIs in `Aspose.Medical.Dicom.DicomFile` and the options in `Aspose.Medical.Dicom.Writers`:
- `Aspose.Medical.Dicom.DicomFile.Save(string, Aspose.Medical.Dicom.Writers.SaveDicomToFileOptions?)`
- `Aspose.Medical.Dicom.DicomFile.Save(System.IO.Stream, Aspose.Medical.Dicom.Writers.SaveDicomToStreamOptions?)`
- `Aspose.Medical.Dicom.DicomFile.Save(System.IO.Pipelines.PipeWriter, bool, Aspose.Medical.Dicom.Writers.SaveDicomToPipeOptions?)`
- `Aspose.Medical.Dicom.DicomFile.Save(System.IO.Pipelines.Pipe, bool, Aspose.Medical.Dicom.Writers.SaveDicomToPipeOptions?)`

## Saving `DICOM` to Files


To save a `DICOM` file to a specific location, call `Aspose.Medical.Dicom.DicomFile.Save(string, Aspose.Medical.Dicom.Writers.SaveDicomToFileOptions?)`.
If the file already exists, it is replaced. When no options are provided, defaults are used.

The following example demonstrates how to save a `DICOM` file to a specified path using default options:

```csharp
// Create an instance of DicomFile (assuming a DICOM file is already created or loaded)
Aspose.Medical.Dicom.DicomFile dicomFile = new();

// Define the output file path
string outputFilePath = "output.dcm";

// Save the DICOM file using default save options
dicomFile.Save(outputFilePath);
```

Save options in `Aspose.Medical.Dicom.Writers.SaveDicomToFileOptions` allow fine-grained control over how the `DICOM` file is saved. These options include:

* `KeepGroupLengths`: Defines whether to retain group length elements.
* `ExplicitLengthSequences`: Specifies if sequences should have an explicit length.
* `ExplicitLengthSequenceItems`: Specifies if sequence items should have an explicit length.

The following example demonstrates how to save a `DICOM` file to a specified path with custom options:

```csharp
// Create an instance of DicomFile (assuming a DICOM file is already created or loaded)
Aspose.Medical.Dicom.DicomFile dicomFile = new();

// Define the output file path
string outputFilePath = "output.dcm";

// Define custom save options for file
Aspose.Medical.Dicom.Writers.SaveDicomToFileOptions options =
    new()
    {
        KeepGroupLengths = false,                 // Remove group length elements
        ExplicitLengthSequences = true,           // Use explicit length for sequences
        ExplicitLengthSequenceItems = true,       // Use explicit length for sequence items
        BufferTuningOptions = new Aspose.Medical.Dicom.Writers.WriteBufferTuningOptions
        {
            MinimumSegmentSizeBytes = 32 * 1024,     // 32 KiB (clamped & 4 KiB–aligned)
            ProducerFlushThresholdBytes = 128 * 1024 // 128 KiB (clamped & 4 KiB–aligned)
        }
    };

// Save the DICOM file using default custom options
dicomFile.Save(outputFilePath, options);
```

## Saving `DICOM` to Streams

It is possible to save a `DICOM` file to a stream by passing an open `System.IO.Stream` to `Save(System.IO.Stream, Aspose.Medical.Dicom.Writers.SaveDicomToStreamOptions?)`. The stream remains open after the call.

```csharp
// Create an instance of DicomFile (assuming a DICOM file is already created or loaded)
Aspose.Medical.Dicom.DicomFile dicomFile = new();

// Create or open a stream
System.IO.MemoryStream memoryStream = new();

// Define custom save options for stream
Aspose.Medical.Dicom.Writers.SaveDicomToStreamOptions options =
    new()
    {
        KeepGroupLengths = false,
        ExplicitLengthSequences = true,
        ExplicitLengthSequenceItems = true,
        BufferTuningOptions = new Aspose.Medical.Dicom.Writers.WriteBufferTuningOptions
        {
            MinimumSegmentSizeBytes = 64 * 1024,      // 64 KiB
            ProducerFlushThresholdBytes = 256 * 1024   // 256 KiB
        }
    };

// Save the DICOM file to the stream
dicomFile.Save(memoryStream, options);
```

## Saving `DICOM` to Pipelines

For producer/consumer scenarios, you can write directly to `System.IO.Pipelines` types.

### Save to `PipeWriter`

```csharp
// Create an instance of DicomFile (assuming a DICOM file is already created or loaded)
Aspose.Medical.Dicom.DicomFile dicomFile = new();

// Create a Pipe and get the writer
System.IO.Pipelines.Pipe pipeForWriter = Create();
System.IO.Pipelines.PipeWriter pipeWriter = pipeForWriter.Writer;

// Define options for pipeline save
Aspose.Medical.Dicom.Writers.SaveDicomToPipeOptions options =
    new()
    {
        KeepGroupLengths = false,
        ExplicitLengthSequences = true,
        ExplicitLengthSequenceItems = true
    };

// Save to PipeWriter; complete writer when disposal happens inside Save
dicomFile.Save(pipeWriter, options);
```

### Save to `Pipe`

```csharp
// Create a Pipe
System.IO.Pipelines.Pipe pipe = Create();

// Use the overload that accepts Pipe directly
dicomFile.Save(pipe, Aspose.Medical.Dicom.Writers.SaveDicomToPipeOptions.Default);
```

## Options

All save options live in `namespace Aspose.Medical.Dicom.Writers;` and implement a common contract for DICOM serialization:

- `KeepGroupLengths` – keep (or remove) group length elements.
- `ExplicitLengthSequences` – write sequences with explicit lengths.
- `ExplicitLengthSequenceItems` – write sequence items with explicit lengths.

Options types (each exposes a `Default` instance):

- `Aspose.Medical.Dicom.Writers.SaveDicomToFileOptions`
- `Aspose.Medical.Dicom.Writers.SaveDicomToStreamOptions`
- `Aspose.Medical.Dicom.Writers.SaveDicomToPipeOptions`

### Buffer tuning for file/stream saves

For file/stream saves, you can fine‑tune write behavior using `Aspose.Medical.Dicom.Writers.WriteBufferTuningOptions` exposed from `SaveDicomToFileOptions.BufferTuningOptions` and `SaveDicomToStreamOptions.BufferTuningOptions`.

```csharp
// Limits (constants)
int minSeg = Aspose.Medical.Dicom.Writers.WriteBufferTuningOptions.MinSegmentSizeBytes;      // 4 KiB
int maxSeg = Aspose.Medical.Dicom.Writers.WriteBufferTuningOptions.MaxSegmentSizeBytes;      // 4 MiB
int minFlush = Aspose.Medical.Dicom.Writers.WriteBufferTuningOptions.MinFlushThresholdBytes; // 4 KiB
int maxFlush = Aspose.Medical.Dicom.Writers.WriteBufferTuningOptions.MaxFlushThresholdBytes; // 16 MiB

// Defaults (values are clamped to ranges above and aligned to 4 KiB)
Aspose.Medical.Dicom.Writers.WriteBufferTuningOptions tuning =
new()
{
    MinimumSegmentSizeBytes = 16 * 1024,
    ProducerFlushThresholdBytes = 64 * 1024
};
```

### Behavior summary

- **File path save:** existing files are deleted before writing the new content.
- **Stream save:** the provided stream remains open.
- **PipeWriter/Pipe saves:** when `completeOnDispose` is `true`, the writer is completed after the save completes; otherwise it is left open for further use.
