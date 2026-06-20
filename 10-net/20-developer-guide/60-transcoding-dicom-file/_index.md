---
title: Transcoding DICOM file
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

## Transcoding to JPEG XL

`Aspose.Medical` supports the JPEG XL transfer syntaxes. Encoding is available for:

- `JpegXLLossless` - lossless JPEG XL (`1.2.840.10008.1.2.4.110`).
- `JpegXL` - lossy or lossless JPEG XL (`1.2.840.10008.1.2.4.112`); lossy mode uses XYB at distance 1.0.

`JpegXLJpegRecompression` (`1.2.840.10008.1.2.4.111`) can be decoded but not yet encoded, so it cannot be used as a transcoding target.

```csharp
// Load existing file
Aspose.Medical.Dicom.DicomFile dcm = DicomFile.Open("input.dcm");

// Transcode the loaded file to JPEG XL Lossless Transfer Syntax.
Aspose.Medical.Dicom.DicomFile transcodedDcm = dcm.Transcode(Aspose.Medical.Dicom.TransferSyntax.JpegXLLossless);

// Save the transcoded file
transcodedDcm.Save("output.dcm");
```

