---
title: Developer Guide
type: docs
weight: 20
url: /open-dicom-file/
---


# Open `DICOM` file

In addition to generating `DICOM` files from scratch, the `Aspose.Medical` library offers a straightforward and effective method for opening and handling `DICOM` files within .NET applications. After you load a `DICOM` file, you can get information about the file, edit the `DICOM` data elements, add new tags, or remove existing ones, etc.

## Opening a `DICOM` File Using a File Path

To open an existing `DICOM` file, you simply have to call `Aspose.Medical.Dicom.DicomFile.Open` method and pass the file path (to the `DICOM` file you want to open) to it.

This `c#` code shows you how to open a `DICOM` file:

```c#
string dcmFilePath = "sample.dcm";
// Open the DICOM file using the specified file path
Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open(dcmFilePath);
```

## Opening a `DICOM` File from a Stream

To open an existing `DICOM` file using its `System.IO.Stream`, you simply have to call the `Aspose.Medical.Dicom.DicomFile.Open` method and pass the stream (with the `DICOM` file you want to open) to it.

This `c#` code shows you how to open a `DICOM` file using its stream:

```c#
// Open the file as a stream
using System.IO.FileStream fileStream = new("sample.dcm", FileMode.Open, FileAccess.Read);
// Open the DICOM file from the stream
Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open(fileStream);
```

## Handling Character Encoding in DICOM Files

Some `DICOM` files may contain non-`ASCII` characters in their data. If the encoding is not specified in the file itself, you can provide a fallback encoding to ensure proper text interpretation.

Both overloads of `Aspose.Medical.Dicom.DicomFile.Open` method allows passing fallback encoding.

This `c#` code shows you how to open a `DICOM` file with fallback encoding:

```c#
string dicomFilePath = "sample.dcm";

// Specify fallback encoding (e.g., UTF-8, shift_jis)
System.Text.Encoding fallbackEncoding = Encoding.GetEncoding("shift_jis");

// Open the DICOM file with fallback encoding
Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open(dicomFilePath, fallbackEncoding);
```
