---
title: Supported Image Data Transfer Syntaxes
type: docs
weight: 20
url: /net/developer-guide/restrictions/supported-image-data-transfer-syntaxes/
---

# Supported Image Data Transfer Syntaxes

> **Note:** Currently, Aspose.Medical for .NET supports only a restricted set of JPEG codecs. All codecs are implemented in pure C# and are platform-independent. Our team is actively working on adding support for additional codecs in future releases.

The following table lists all standard DICOM transfer syntaxes and their current support status in Aspose.Medical for .NET:

| Transfer Syntax Name                                                                    | UID                     | Description                                 | Support Status | Notes                                                 |
| --------------------------------------------------------------------------------------- | ----------------------- | ------------------------------------------- | -------------- | ----------------------------------------------------- |
| **Uncompressed Transfer Syntaxes**                                                      |                         |                                             |                |                                                       |
| Implicit VR Little Endian                                                               | 1.2.840.10008.1.2       | Default DICOM transfer syntax               |                |                                                       |
| Explicit VR Little Endian                                                               | 1.2.840.10008.1.2.1     | Explicit VR with little endian              |                |                                                       |
| Encapsulated Uncompressed Explicit VR Little Endian                                     | 1.2.840.10008.1.2.1.98  | Encapsulated uncompressed                   |                |                                                       |
| Deflated Explicit VR Little Endian                                                      | 1.2.840.10008.1.2.1.99  | Deflated explicit VR                        |                |                                                       |
| **JPEG Baseline Process 1 Transfer Syntaxes**                                           |                         |                                             |                |                                                       |
| JPEG Baseline (Process 1)                                                               | 1.2.840.10008.1.2.4.50  | Lossy JPEG 8-bit                            |                |                                                       |
| JPEG Extended (Process 2 & 4)                                                           | 1.2.840.10008.1.2.4.51  | Lossy JPEG 12-bit                           | Not Supported  | Currently not implemented in Aspose.Medical for .NET  |
| **JPEG Lossless Transfer Syntaxes**                                                     |                         |                                             |                |                                                       |
| JPEG Lossless, Non-Hierarchical (Process 14)                                            | 1.2.840.10008.1.2.4.57  | Lossless JPEG                               |                | Support limited to 8-bit images only                  |
| JPEG Lossless, Non-Hierarchical, First-Order Prediction (Process 14, Selection Value 1) | 1.2.840.10008.1.2.4.70  | Lossless JPEG                               |                | Support limited to 8-bit images only                  |
| **JPEG-LS Transfer Syntaxes**                                                           |                         |                                             |                |                                                       |
| JPEG-LS Lossless                                                                        | 1.2.840.10008.1.2.4.80  | Lossless JPEG-LS                            |                |                                                       |
| JPEG-LS Near-Lossless                                                                   | 1.2.840.10008.1.2.4.81  | Near-lossless JPEG-LS                       |                |                                                       |
| **JPEG 2000 Transfer Syntaxes**                                                         |                         |                                             |                |                                                       |
| JPEG 2000 Lossless Only                                                                 | 1.2.840.10008.1.2.4.90  | Lossless JPEG 2000                          |                | Supports 8-bit and 16-bit reading, only 8-bit writing |
| JPEG 2000                                                                               | 1.2.840.10008.1.2.4.91  | Lossy or lossless JPEG 2000                 |                | Supports 8-bit and 16-bit reading, only 8-bit writing |
| JPEG 2000 Part 2 Multicomponent Lossless Only                                           | 1.2.840.10008.1.2.4.92  | Lossless JPEG 2000 multi-component          |                | Supports 8-bit and 16-bit reading, only 8-bit writing |
| JPEG 2000 Part 2 Multicomponent                                                         | 1.2.840.10008.1.2.4.93  | Lossy or lossless JPEG 2000 multi-component |                | Supports 8-bit and 16-bit reading, only 8-bit writing |
| **JPEG XL Transfer Syntaxes**                                                           |                         |                                             |                |                                                       |
| JPEG XL Lossless                                                                        | 1.2.840.10008.1.2.4.110 | Lossless JPEG XL                            | Not Supported  | Currently not implemented in Aspose.Medical for .NET  |
| JPEG XL JPEG Recompression                                                              | 1.2.840.10008.1.2.4.111 | JPEG XL JPEG Recompression                  | Not Supported  | Currently not implemented in Aspose.Medical for .NET  |
| JPEG XL                                                                                 | 1.2.840.10008.1.2.4.112 | Lossy or lossless JPEG XL                   | Not Supported  | Currently not implemented in Aspose.Medical for .NET  |
| **High-Throughput JPEG 2000 Transfer Syntaxes**                                         |                         |                                             |                |                                                       |
| High-Throughput JPEG 2000 Image Compression (Lossless Only)                             | 1.2.840.10008.1.2.4.201 | HTJ2K lossless                              | Not Supported  | Currently not implemented in Aspose.Medical for .NET  |
| High-Throughput JPEG 2000 with RPCL Options Image Compression (Lossless Only)           | 1.2.840.10008.1.2.4.202 | HTJ2K lossless with RPCL                    | Not Supported  | Currently not implemented in Aspose.Medical for .NET  |
| High-Throughput JPEG 2000 Image Compression                                             | 1.2.840.10008.1.2.4.203 | HTJ2K lossy or lossless                     | Not Supported  | Currently not implemented in Aspose.Medical for .NET  |
| **RLE Transfer Syntax**                                                                 |                         |                                             |                |                                                       |
| RLE Lossless                                                                            | 1.2.840.10008.1.2.5     | Run Length Encoding                         |                |                                                       |
