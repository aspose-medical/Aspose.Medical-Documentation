---
title: Open DICOM file
type: docs
weight: 10
url: /net/developer-guide/open-dicom-file/
---


# Open `DICOM` file

The `Aspose.Medical` library offers a straightforward and effective method for opening and handling `DICOM` files within .NET applications. Opening existing DICOM files is the most common starting point for medical data workflows. After you load a `DICOM` file, you can get information about the file, edit the `DICOM` data elements, add new tags, or remove existing ones, etc.

## Opening a `DICOM` File Using a File Path

To open an existing `DICOM` file, you simply have to call `Aspose.Medical.Dicom.DicomFile.Open` method and pass the file path (to the `DICOM` file you want to open) to it.

This `c#` code shows you how to open a `DICOM` file:

```csharp

// Optional: tune file I/O buffers and fallback text encoding
Aspose.Medical.Dicom.Readers.ReadDicomFileOptions options = ReadDicomFileOptions.Default with
{
    FallbackEncoding = Encoding.GetEncoding("shift_jis"),
    BufferTuningOptions = ReadBufferTuningOptions.Default
};

string dcmFilePath = "sample.dcm";
// Open the DICOM file using the specified file path
Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open(dcmFilePath, options);
```

*Exceptions*:
- `ArgumentException` (null/whitespace path),
- `FileNotFoundException` (path not found),
- `BadDicomFileException` (not a valid DICOM file).


## Opening a `DICOM` File from a Stream

To open an existing `DICOM` file using its `System.IO.Stream`, you simply have to call the `Aspose.Medical.Dicom.DicomFile.Open` method and pass the stream (with the `DICOM` file you want to open) to it.

This `c#` code shows you how to open a `DICOM` file using its stream:

```csharp
// Optional: tune stream PipeReader buffers and fallback text encoding
Aspose.Medical.Dicom.Readers.ReadDicomStreamOptions options = ReadDicomStreamOptions.Default with
{
    FallbackEncoding = System.Text.Encoding.UTF8,
    BufferTuningOptions = Aspose.Medical.Dicom.Readers.ReadBufferTuningOptions.Default
};

Aspose.Medical.Dicom.Readers.ITagDataReadingStrategy strategy = Aspose.Medical.Dicom.Readers.TagDataReadingStrategies.ReadAll();

await using System.IO.FileStream fs = new("sample.dcm", System.IO.FileMode.Open, System.IO.FileAccess.Read);

Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open(fs, options, strategy);
```

*Exception*: `BadDicomFileException` if the stream does not contain a valid DICOM file.

## Opening a `DICOM` File from Pipe

To open an existing `DICOM` file using `Pipe`, you simply have to call the `Aspose.Medical.Dicom.DicomFile.Open` method and pass the pipe (with the `DICOM` file you want to open) to it.

This `c#` code shows you how to open a `DICOM` file using pipe:

```csharp
Aspose.Medical.Dicom.Readers.ReadDicomPipeOptions options = ReadDicomPipeOptions.Default with
{
    FallbackEncoding = System.Text.Encoding.UTF8,
};

Aspose.Medical.Dicom.Readers.ITagDataReadingStrategy strategy = TagDataReadingStrategies.ReadAll();

System.IO.Pipelines.Pipe pipe; // pipe with DICOM file

Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open(pipe, options, strategy);

```

*Exception*: `BadDicomFileException` if the stream does not contain a valid DICOM file.

## Opening a `DICOM` File from Pipe Reader

To open an existing `DICOM` file using `Pipe`, you simply have to call the `Aspose.Medical.Dicom.DicomFile.Open` method and pass the pipe (with the `DICOM` file you want to open) to it.

This `c#` code shows you how to open a `DICOM` file using pipe:

```csharp
Aspose.Medical.Dicom.Readers.ReadDicomPipeOptions options = ReadDicomPipeOptions.Default with
{
    FallbackEncoding = System.Text.Encoding.GetEncoding("shift_jis"),
};

Aspose.Medical.Dicom.Readers.ITagDataReadingStrategy strategy = TagDataReadingStrategies.ReadAll();

System.IO.Pipelines.PipeReader pipeReader; // pipe with DICOM file

Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open(pipeReader, options, strategy);

```

*Exception*: `BadDicomFileException` if the stream does not contain a valid DICOM file.


## Buffer tuning

When opening from file/stream, you can tune the underlying buffers via `ReadBufferTuningOptions` on:

- `ReadDicomFileOptions.BufferTuningOptions`
- `ReadDicomStreamOptions.BufferTuningOptions`

`ReadBufferTuningOptions` allows to specify:
- `MinimumReadSize`. Low-watermark in bytes before refilling. Default value is -1. Clamped to [4 KiB, 4 MiB], aligned up to 4 KiB.
- `BufferSize`. Minimum buffer size (bytes) when renting from the pool. Default value is -1. Clamped to [4 KiB, 4 MiB], aligned up to 4 KiB.

For pipe inputs, the producer typically controls buffer sizes and flush cadence; `ReadDicomPipeOptions` exposes only FallbackEncoding.


## Handling Character Encoding in DICOM Files

Some `DICOM` files may contain non-`ASCII` characters in their data. If the encoding is not specified in the file itself, you can provide a fallback encoding to ensure proper text interpretation.

Both overloads of `Aspose.Medical.Dicom.DicomFile.Open` method allows passing fallback encoding.

This `c#` code shows you how to open a `DICOM` file with fallback encoding:

```csharp
string dicomFilePath = "sample.dcm";

System.Text.Encoding fallbackEncoding = Encoding.GetEncoding("shift_jis");

// Specify fallback encoding (e.g., UTF-8, shift_jis)
Aspose.Medical.Dicom.Readers.ReadDicomFileOptions options = ReadDicomFileOptions.Default with
{
    FallbackEncoding = fallbackEncoding,
    BufferTuningOptions = ReadBufferTuningOptions.Default
};


// Open the DICOM file with fallback encoding
Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open(dicomFilePath, options);
```

# Advanced Options
The Aspose.Medical.Dicom library offers several advanced strategies for reading data from DICOM files efficiently. This is especially useful when dealing with large datasets where memory optimization is critical. You can find additional information [here]({{< ref "/10-net/20-developer-guide/10-open-dicom-file/10-memory-management" >}} "Memory Management").