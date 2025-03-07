---
title: Developer Guide
type: docs
weight: 60
url: /net/developer-guide/transcoding-dicom-file/
---


# Transcoding `DICOM` file

`DICOM` files use various *Transfer Syntaxes*, which define how image data is encoded. To ensure compatibility across different systems, you may need to transcode (convert) a DICOM file from one transfer syntax to another.
`Aspose.Medical` provides a simple API to transcode `DICOM` files efficiently.

To transcode a `DICOM` file, please follow the steps below:
1. Load an existing `DICOM` file.
2. To transcode the `DICOM` file to another format, use the `Aspose.Medical.Dicom.DicomFile.Transcode()` method.
3. Save the transcoded file for further use.

```csharp
// Load existing file
Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open("input.dcm");

// Transcode the loaded file to Jpeg 2000 Lossy Transfer Syntax.
Aspose.Medical.Dicom.DicomFile transcodedDcm = dcm.Transcode(Aspose.Medical.Dicom.TransferSyntax.Jpeg2000Lossy);

// Save the transcoded file
transcodedDcm.Save("output.dcm");
```

