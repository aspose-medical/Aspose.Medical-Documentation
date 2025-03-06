---
title: Developer Guide
type: docs
weight: 50
url: /net/developer-guide/dicom-imaging-operations/
---

# DICOM Imaging Operations

This documentation provides a step-by-step guide on how to render images (frames) stored in a `DICOM` file using the **`Aspose.Medical`** library. The **`Aspose.Medical.Dicom`** `API` allows us to load a `DICOM` file and render individual frames as raw images for further processing or display.

## Checking the Number of Frames

`DICOM` files may contain **multiple frames** (e.g., for CT or MRI scans). To determine how many frames are present, use the `NumberOfFrames` property.

```csharp
// Load DICOM file
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom-file.dcm");

int frameCount = dicomFile.NumberOfFrames;
System.Console.WriteLine($"Total Frames in DICOM file: {frameCount}");
```

This information helps decide which frame to render.

## Rendering a Single Frame as an Image

The `Aspose.Medical.Dicom.DicomFile.RenderImage()` method allows rendering a single image (frame) from the `DICOM` file. By default, it renders the **first frame (index 0)**.

```csharp
// Load DICOM file
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom-file.dcm");

Aspose.Medical.Imaging.IRawImage rawImage = dicomFile.RenderImage(0);
```
The `Aspose.Medical.Imaging.IRawImage` contains image pixel data (in BGRA-32 format), dimensions, and color information.

### Accessing Image Properties

After rendering, we can inspect the generated image details:

```csharp
int width = rawImage.Width;
int height = rawImage.Height;

System.Console.WriteLine($"Rendered Frame Dimensions: {width} x {height}");
```
This gives us the size of the rendered image.

### Accessing Pixel Values

```csharp
Aspose.Medical.Imaging.PixelFormats.Bgra32 pixelColor = rawImage[10, 10]; // Get pixel at (10,10)
System.Console.WriteLine($"Pixel at (10,10): R={pixelColor.R}, G={pixelColor.G}, B={pixelColor.B}");
```

This can be useful for **image analysis** or **displaying pixels** with custom processing.

## Rendering a Specific Frame

If the `DICOM` file contains multiple frames, we can specify which frame to render by its **zero-based index**.

```csharp
// Load DICOM file
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom-file.dcm");

 // Render the frame # 43 (index 42)
int frameIndex = 42;
Aspose.Medical.Imaging.IRawImage rawImage = dicomFile.RenderImage(frameIndex);
```
This is useful for multi-frame modalities like **CT and MRI** scans.

## Rendering a Frame with Custom Options

We can customize the rendering process using `Aspose.Medical.Dicom.Imaging.Options.RenderOptions`. These options control:

- **Overlay visibility**
- **Overlay color**

To render image with Overlays Disabled please use the following snippet:

```csharp
// Load DICOM file
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom-file.dcm");

Aspose.Medical.Dicom.Imaging.Options.RenderOptions options = new()
{
    ShowOverlays = true,  // Ensure overlays are visible
    OverlayColor = Aspose.Medical.Imaging.PixelFormats.Bgra32.Black, // Set overlay color to black
};

Aspose.Medical.Imaging.IRawImage rawImage = dicomFile.RenderImage(options, 0);
```

## Iterating Over All Frames in a Multi-Frame `DICOM` File

Some `DICOM` files contain multiple image frames. You can iterate over all frames using `DicomFile.NumberOfFrames` and render each one sequentially.

### Steps:

1. Open the DICOM file.
2. Determine the total number of frames using `NumberOfFrames`.
3. Loop through each frame and render it using `RenderImage(int frameIndex)`.

```csharp
// Open the DICOM file
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom-file.dcm");

// Get the total number of frames in the DICOM file
int totalFrames = dicomFile.NumberOfFrames;
Console.WriteLine($"Total Frames: {totalFrames}");

// Iterate over all frames and render each one
for (int i = 0; i < totalFrames; i++)
{
    IRawImage image = dicomFile.RenderImage(i);
    Console.WriteLine($"Rendered Frame {i + 1}: {image.Width}x{image.Height}");
}
```

## Accessing Raw Pixel Data

To access the array with the raw values of image data you can use `Aspose.Medical.Dicom.Imaging.PixelData` class. Please use the following snippet to access raw image data:

```csharp
// Open the DICOM file
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom-file.dcm");

// Reads pixel data from Dataset
Aspose.Medical.Dicom.Imaging.PixelData pixelData = PixelData.Read(dicomFile.Dataset);

// Raw image data available using GetRawData method.
System.Span<byte> rawImageData = pixelData.GetRawData();
```
