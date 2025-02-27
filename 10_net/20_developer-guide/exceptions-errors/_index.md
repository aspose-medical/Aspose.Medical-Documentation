---
title: Developer Guide
type: docs
weight: 20
url: /exceptions-errors/
---


# Exceptions/Errors

`Aspose.Medical` provides a set of custom exceptions to handle various errors related to `DICOM` file processing and validation.

## Exception Hierarchy

`Aspose.Medical` defines several exceptions that extend from `System.Exception` to represent specific failure scenarios.

```
System.Exception
│
└── Aspose.Medical.Errors.MedicalApiException
    │
    ├── Aspose.Medical.Dicom.Errors.BadDicomFileException
    ├── Aspose.Medical.Dicom.ValueValidation.DicomValidationException
```

## Understanding the Exception Classes

### `MedicalApiException`
```c#
Aspose.Medical.Errors.MedicalApiException
```

This is the base class for all exceptions related to DICOM file processing in `Aspose.Medical`.

### `BadDicomFileException`
```c#
Aspose.Medical.Dicom.Errors.BadDicomFileException
```

This specific exception is thrown when a DICOM file is corrupted or incorrectly formatted.

### `DicomValidationException`

```c#
Aspose.Medical.Dicom.ValueValidation.DicomValidationException
```

This exception indicates that validation of DICOM file contents failed due to non-conformant data.

## Basic usage

The following example demonstrates how to correctly handle exceptions when working with `DICOM` files using `Aspose.Medical`:

```c#
try
{
    ProcessDicomFile("invalid.dcm");
}
catch (Aspose.Medical.Dicom.Errors.BadDicomFileException ex)
{
    // Handle the case where the file is invalid or corrupted.
    Console.WriteLine("Error: The DICOM file is corrupted or has an invalid format.");
    Console.WriteLine($"Details: {ex.Message}");
}
catch (Aspose.Medical.Dicom.ValueValidation.DicomValidationException ex)
{
    // Handle validation issues in the DICOM file.
    Console.WriteLine("Error: DICOM file validation failed due to incorrect data.");
    Console.WriteLine($"Details: {ex.Message}");
}
catch (Aspose.Medical.Errors.MedicalApiException ex)
{
    // Handle any other generic DICOM-related errors.
    Console.WriteLine("Error: A general issue occurred while processing the DICOM file.");
    Console.WriteLine($"Details: {ex.Message}");
}
```

1. Catching `BadDicomFileException` First
    1. This is the most specific exception.
    2. It is caught first to handle specific corrupt file scenarios.
2. Catching `DicomValidationException` Next
    1. This exception handles cases where a DICOM file doesn't conform to required standards.
    2. It is placed after `BadDicomFileException` as it is slightly broader.
3. Catching `MedicalApiException`
    1. Any generic DICOM-related errors are caught here.
    2. Since all other specific exceptions inherit from `MedicalApiException`, this ensures any other case is covered.
