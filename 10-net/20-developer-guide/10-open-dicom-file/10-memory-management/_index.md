---
title: Memory Management
type: docs
weight: 10
url: /net/developer-guide/open-dicom-file/memory-management
---


# Optimized Reading of Large `DICOM` Files

The `Aspose.Medical.Dicom` library offers several advanced strategies for reading data from `DICOM` files efficiently. This is especially useful when dealing with large datasets where memory optimization is critical.

## Overview

`DICOM` files may contain large tag values (e.g., image pixel data or long annotations) that can lead to excessive memory usage if loaded all at once. To address this, the library introduces memory-optimized reading strategies to control how tag data is accessed.

## Key Concepts

### Tag Data Reading Strategy

The tag data reading strategy defines how `DICOM` tags are loaded into memory. The `Aspose.Medical.Dicom.Readers.TagDataReadingStrategies` class provides three built-in strategies:

| Strategy | Description |
|---------|-------------|
| ReadAll | Eagerly loads all tag data into memory. |
| ReadLargeOnDemand | Loads large tags only when explicitly accessed. |
| SkipLargeTags | Skips reading large tags entirely. |

---

## API Usage

### Load Full `DICOM` File (All Tags Immediately)

This strategy is suitable for small to moderate DICOM files.

```csharp
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open(
    "file.dcm",
    fallbackEncoding: System.Text.Encoding.UTF8,
    strategy: Aspose.Medical.Dicom.Readers.TagDataReadingStrategies.ReadAll()
);
```

### Load DICOM File with Deferred Loading of Large Tags

This strategy loads all small tags immediately but defers the loading of large data until explicitly accessed. Ideal for large `DICOM` files when you don't always need to use all tag data.

```csharp
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open(
    "large_file.dcm",
    fallbackEncoding: System.Text.Encoding.UTF8,
    strategy: Aspose.Medical.Dicom.Readers.TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 128) // Tags > 128 KB will be deferred
);
```

Later, when you access a large tag, it will be loaded on demand.
```csharp
// Example: Access large sequence
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;
Span<byte> pixelDataTag = dataset.GetValues<byte>(Aspose.Medical.Dicom.Tags.Tag.PixelData);
```

### Skip Loading Large Tags

For scenarios where memory footprint must be minimized, and large tag data is not needed.

```csharp
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open(
    "large_file.dcm",
    fallbackEncoding: System.Text.Encoding.UTF8,
    strategy: Aspose.Medical.Dicom.Readers.TagDataReadingStrategies.SkipLargeTags(largeObjectSizeKb: 256) // Skip tags > 256 KB
);
```

---

## Threshold Configuration

For both `ReadLargeOnDemand()` and `SkipLargeTags()`, you can customize the size threshold:

- Parameter: `largeObjectSizeKb` (type: `int`)
- Default: 64 kilobytes
- Usage:

  - A tag is considered “large” if its value is greater than or equal to the threshold.
  - If `0` or not specified, the default 64 KB threshold will apply.
  - Values are clamped internally to be ≥ 0 and ≤ `int.MaxValue`.

---


## Advanced: Load `DICOM` from Stream

All strategies work with stream-based file access too.

```csharp
using System.IO.FileStream fileStream = File.OpenRead("file.dcm");
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open(
    fileStream,
    fallbackEncoding: System.Text.Encoding.UTF8,
    strategy: Aspose.Medical.Dicom.Readers.TagDataReadingStrategies.ReadLargeOnDemand(largeObjectSizeKb: 512)
);
```

---

## Strategy Summary

| Strategy Method                                              | Memory Usage | Load Time | Access Speed |
|--------------------------------------------------------------|--------------|-----------|--------------|
| ReadAll()                                                    | High         | Slow      | Fast         |
| ReadLargeOnDemand(int largeObjectSizeKb = 64)                | Medium       | Medium    | Medium       |
| SkipLargeTags(int largeObjectSizeKb = 64)                    | Low          | Fast      | N/A (skipped)|

---

## Default Behavior

If `strategy` is not specified, the default behavior is to read all tags. It is strongly recommended to specify an appropriate strategy explicitly when dealing with large files.

---

## Best Practices

- Use `ReadAll` only for small to medium `DICOM` files where memory is not constrained.
- For large imaging files (MRI/CT), prefer `ReadLargeOnDemand` to conserve memory.
- Use `SkipLargeTags` if tag data is not needed (e.g., when inspecting metadata only).
