---
title: Getting DICOM data
type: docs
weight: 10
url: /net/developer-guide/dicom-data-manipulation/getting-dicom-data/
---


# Getting DICOM data

`Aspose.Medical` provides an easy-to-use API for reading and extracting information from `DICOM` files.

## Checking for Data Availability

There could be scenarios when you need to check if the dataset contains the required tag before retrieving specific data from a `DICOM` file. Method `Aspose.Medical.Dicom.Dataset.Contains` allows checking if data is available or not.

```csharp
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom/file.dcm");
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;

// Define the DICOM tag for Patient Name (0010,0010)
Aspose.Medical.Dicom.Tags.Tag patientNameTag = Tag.PatientName;

// Check if the dataset contains the tag
if (dataset.Contains(patientNameTag))
{
    Console.WriteLine("Patient Name tag is available.");
}
else
{
    Console.WriteLine("Patient Name tag is not found in the DICOM file.");
}
```

## Retrieving Single Values from a `DICOM` File

Some `DICOM` tags have only one value, such as the patient's name or study description. Use `Aspose.Medical.Dicom.Dataset.GetSingleValue<T>` to retrieve a single value. The method enforces the constraint and throws an exception when it’s not met.

```csharp
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom/file.dcm");
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;

// Define the tag for Patient's Name
Aspose.Medical.Dicom.Tags.Tag patientNameTag = Tag.PatientName;

// Retrieve the patient's name
string patientName = dataset.GetSingleValue<string>(patientNameTag);
```

**NOTE**: The method fails with an exception if the specified tag is absent in the dataset.

## Retrieving Multiple Values from a `DICOM` File

Some `DICOM` tags contain multiple values—for example, Image Positions or Pixel Spacing. Use `Aspose.Medical.Dicom.Dataset.GetValues<T>` to retrieve multiple values.

```csharp
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom/file.dcm");
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;

// Define the tag for Pixel Spacing
Aspose.Medical.Dicom.Tags.Tag pixelSpacingTag = Tag.PixelSpacing;

// Retrieve multiple values
System.Span<double> pixelSpacingValues = dataset.GetValues<double>(pixelSpacingTag);

Console.WriteLine("Pixel Spacing:");
foreach (double value in pixelSpacingValues)
{
    Console.WriteLine(value);
}
```

**NOTE**: The method fails with an exception if the specified tag is absent in the dataset. The method `Aspose.Medical.Dicom.Dataset.FindValues` returns an empty `System.Span<T>` if the tag is absent.

## Retrieving a Default Value if a `Tag` is Missing

Sometimes, a `DICOM` tag may be missing from the file. The `Aspose.Medical.Dicom.Dataset.GetSingleValueOrDefault<T>` method allows you to specify a default value when the tag is absent.

```csharp
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom/file.dcm");
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;

// Define the tag for Institution Name (0008,0080)
Aspose.Medical.Dicom.Tags.Tag institutionNameTag = Tag.InstitutionName;

// Retrieve the institution name or return "Unknown" if not found
string institutionName = dataset.GetSingleValueOrDefault<string>(institutionNameTag, "Unknown");

Console.WriteLine("Institution Name: " + institutionName);
```

**NOTE**: The method fails with an exception if the specified tag is absent in the dataset.

## Retrieving a Specific Value from a Multi-Valued `Tag`

For For DICOM tags that contain multiple values, use the `Aspose.Medical.Dicom.Dataset.GetValue<T>` method to access a specific value at a given index. tags that contain multiple values, use the `Aspose.Medical.Dicom.Dataset.GetValue<T>` method to access a specific value at a given index.

```csharp
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom/file.dcm");
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;

// Define the tag for Image Position
Aspose.Medical.Dicom.Tags.Tag imagePositionTag = Tag.ImagePositionPatient;

// Retrieve the first value from the Image Position Patient array
double imagePositionX = dataset.GetValue<double>(imagePositionTag, ^1);

Console.WriteLine("First Image Position X: " + imagePositionX);
```

**NOTE**: The method fails with an exception if the specified tag is absent in the dataset.

## Using TryGet Methods for Safe Data Retrieval

`Aspose.Medical` provides `Aspose.Medical.Dicom.Dataset.TryGetSingleValue<T>` to safely retrieve data without raising an exception if the tag does not exist.

```csharp
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("path/to/dicom/file.dcm");
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;

// Try to get the patient's name safely
Aspose.Medical.Dicom.Tags.Tag tag = Tag.PatientName;
if (dataset.TryGetSingleValue(tag, out string? patientName))
{
    // Value exists - work with it
}
else
{
    // The tag was not found in the dataset
}
```