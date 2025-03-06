---
title: Developer Guide
type: docs
weight: 20
url: /net/developer-guide/create-dicom-file/
---

# Create `DICOM` file

`Aspose.Medical` provides the `Aspose.Medical.Dicom.DicomFile` class, which allows users to create, modify, and work with `DICOM` files. To create `Aspose.Medical.Dicom.DicomFile` from scratch, you can simply initialize a new instance of the `Aspose.Medical.Dicom.DicomFile` class using any available constructor.

## Create `DICOM` file from scratch

For scenarios where an empty `DICOM` file needs to be initialized before adding data, use the parameterless constructor.

```csharp
// Create an empty DICOM file
Aspose.Medical.Dicom.DicomFile dicomFile = new();

// Add data to the newly created DICOM file
dicomFile.Dataset.AddOrUpdate(Tag.XAAcquisitionFrameRate, 17.95);
```

## Create `DICOM` file in `.NET` based on existing data

If you have an existing `DICOM` file, you can create a new `Aspose.Medical.Dicom.DicomFile` instance by copying data from another `Aspose.Medical.Dicom.DicomFile` object. Aspose.Medical provides a few overloads of the `Aspose.Medical.Dicom.DicomFile` constructor that allow copying data from datasets, metadata, and the whole `DICOM` file.

### Creating a `DICOM` File using an Existing `DICOM` File

If you have an existing `DICOM` file, you can create a new `Aspose.Medical.Dicom.DicomFile` instance by copying data from another `Aspose.Medical.Dicom.DicomFile` object.

```csharp
// Load an existing DICOM file
Aspose.Medical.Dicom.DicomFile existingFile = LoadExistingDicomFile();

// Create a copy of the DICOM file
Aspose.Medical.Dicom.DicomFile dicomFile = new(existingFile);

// Add data to the newly created DICOM file
dicomFile.Dataset.AddOrUpdate(Tag.XAAcquisitionFrameRate, 17.95);
```

### Creating a DICOM File from a Dataset

If you want to create a new `DICOM` file from scratch using a `Aspose.Medical.Dicom.Dataset`, you can use the `Aspose.Medical.Dicom.DicomFile(Aspose.Medical.Dicom.Dataset dataset)` constructor.

```csharp
// Create a new dataset that will hold medical imaging data
Aspose.Medical.Dicom.Dataset dataset = [];

// Create a new DICOM file from the dataset
Aspose.Medical.Dicom.DicomFile dicomFile = new(dataset);

// Add data to the newly created DICOM file
dicomFile.Dataset.AddOrUpdate(Tag.XAAcquisitionFrameRate, 17.95);
```

### Creating a DICOM File with Custom Metadata and Dataset

If you need to customize the metadata along with the dataset, use the constructor that accepts `Aspose.Medical.Dicom.MetaInformation` and `Aspose.Medical.Dicom.Dataset`.

```csharp
// Create a new dataset for storing DICOM attributes
Aspose.Medical.Dicom.Dataset dataset = [];

// Create meta information for the DICOM file
Aspose.Medical.Dicom.MetaInformation metaInfo = [];

// Create a DICOM file with custom metadata and dataset
Aspose.Medical.Dicom.DicomFile dicomFile = new(metaInfo, dataset);

// Add data to the newly created DICOM file
dicomFile.Dataset.AddOrUpdate(Tag.XAAcquisitionFrameRate, 17.95);
```
