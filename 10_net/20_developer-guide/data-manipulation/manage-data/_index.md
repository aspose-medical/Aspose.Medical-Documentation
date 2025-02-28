---
title: Developer Guide
type: docs
weight: 20
url: /net/developer-guide/data-manipulation/manage-data/
---

# Adding, Updating, and Removing Data from `DICOM` Files using `Aspose.Medical`

`DICOM` (Digital Imaging and Communications in Medicine) is a standard for handling, storing, printing, and transmitting medical imaging data. `Aspose.Medical` provides an easy-to-use API to manipulate `DICOM` files, including adding, updating, and removing data elements.

This guide covers how to work with `DICOM` datasets using `Aspose.Medical.Dicom.Dataset` to manage `DICOM` elements efficiently.

### Key Takeaways:
- **Use `AddOrUpdate()`** to add or modify individual DICOM elements.
- **Use `Remove()`** to delete unwanted DICOM elements.
- **Use `AddOrUpdateRange()`** for bulk additions or updates.
- **Use predicates in `Remove()`** to dynamically clear elements that match specific conditions.

## Adding Data to a `DICOM` File

You can add new elements to a `DICOM` file using the `Aspose.Medical.Dicom.Dataset.AddOrUpdate()` method. This method ensures that if the element already exists, it will be updated. To add or update `DICOM` element data, please use the code snippet below:

```csharp
// Access the Dataset from the DICOM file
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;

// Define the DICOM tag for patient's name
Aspose.Medical.Dicom.Tags.Tag patientNameTag = Tag.PatientName;

// Patient Name to be set
string patientName = "John Doe";

// Add or update the patient's name
dataset.AddOrUpdate(patientNameTag, patientName);
```

## Updating Existing Data in a `DICOM` File

Updating data follows the same process as adding new data. If a tag already exists, `Aspose.Medical.Dicom.Dataset.AddOrUpdate()` will replace its value with the provided new value. To update `DICOM` element data, please use the code snippet below:

```csharp
// Retrieve the DICOM dataset
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;

// Use the tag for Patient's Age
Aspose.Medical.Dicom.Tags.Tag patientAgeTag = Tag.PatientAge;

// Age to be set
Aspose.Medical.Dicom.Elements.Age newAge = new() { Number = 42, Units = Age.Unit.Years };

// Update the age in the dataset
dataset.AddOrUpdate(patientAgeTag, newAge);
```
## Removing Data from a `DICOM` File

You can remove elements from a dataset using the `Aspose.Medical.Dicom.Dataset.Remove()` method. Removing unnecessary or incorrect elements is useful for maintaining data integrity. To remove `DICOM` element, please use the code snippet below:

```csharp
// Retrieve the dataset
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;

// Define the tag for Patient's Address
Aspose.Medical.Dicom.Tags.Tag patientAddressTag = Tag.PatientAddress;

// Remove the element from the dataset
dataset.Remove(patientAddressTag);
```

`Aspose.Medical` provides an overload of `Remove` method that allows specifying multiple tags to be removed. The following code snippet demonstrates how to use it:

```csharp
// Retrieve the dataset
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;

// Remove the elements from the dataset
dataset.Remove(
    Aspose.Medical.Dicom.Tags.Tag.PatientName,
    Aspose.Medical.Dicom.Tags.Tag.AITDeviceTypeRETIRED,
    Aspose.Medical.Dicom.Tags.Tag.XRayOutput
);
```

## Adding Multiple Elements at Once

If you need to update multiple tags simultaneously, the dataset provides overloads for handling a batch of updates.

```csharp
// Retrieve the DICOM dataset
Aspose.Medical.Dicom.Dataset dataset = dicomFile.Dataset;

// Create a list of DICOM elements
Aspose.Medical.Dicom.Elements.IElement[] elements =
[
    new PersonName(Tag.PatientName, ["John Doe"]),
    new LongString(Tag.PatientID, ["64AD6A3F"]),
    new Date(Tag.PatientBirthDate, [new DateOnly(2002, 10, 10)])
];

// Add all elements to the dataset at once
dataset.AddOrUpdateRange(elements);
```

## Removing Multiple Elements Based on a Condition

Sometimes it is necessary to remove multiple records that match a condition.

```csharp
ushort tagGroupToRemove = 0x2;

// Retrieve the dataset
Dataset dataset = dicomFile.Dataset;

// Remove all elements where the value contains the given string
dataset.Remove(element => element.Tag.Group == tagGroupToRemove);
```
