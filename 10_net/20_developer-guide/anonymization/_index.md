---
title: Developer Guide
type: docs
weight: 20
url: /net/developer-guide/anonymization/
---


# Anonymization

`DICOM` (Digital Imaging and Communications in Medicine) files contain sensitive patient information that must be protected when sharing medical imaging data. Anonymization ensures that personally identifiable information (PII) is removed or modified while maintaining the integrity of the medical images.

`Aspose.Medical.Dicom.Anonymization` provides an `API` to anonymize `DICOM` files using predefined confidentiality profiles, ensuring compliance with privacy regulations such as `HIPAA` and `GDPR`.

## Anonymizing a `DICOM` File (Basic Usage)

You can anonymize `DICOM` files in a few simple steps.
The `Aspose.Medical.Dicom.Anonymization.Anonymizer.Anonymize` method removes or redacts sensitive patient and study information while preserving the structure of the `DICOM` dataset.
To anonymize a `DICOM` file, please follow the steps below:
1. Load an existing `DICOM` file using `DicomFile.Open`.
2. Create an anonymizer instance (`Aspose.Medical.Dicom.Anonymization.Anonymizer`) with default settings.
3. Use `anonymizer.Anonymize(dicomFile)` to remove sensitive attributes.
4. Save the anonymized file for further use.

```csharp
// Load a DICOM file
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create an anonymizer with the default profile
Aspose.Medical.Dicom.Anonymization.Anonymizer anonymizer = new();

// Anonymize the DICOM file
Aspose.Medical.Dicom.DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("anonymized_output.dcm");
```

## Anonymization Profiles

Anonymization profiles allow customized redaction of `DICOM` metadata based on predefined rules. Profiles define how specific metadata fields should be handled.
A confidentiality profile helps control what information gets anonymized or retained. The library supports loading profiles from various formats (`CSV`, `JSON`, `XML`).

To anonymize a `DICOM` file using a predefined specific Confidential Profile, please follow the steps below:
1. Initializes the anonymization profile using `ConfidentialityProfile.CreateDefault` method with the specific options, e.g., `ConfidentialityProfileOptions`.CleanGraph.
2. Create an anonymizer instance (`Aspose.Medical.Dicom.Anonymization.Anonymizer`) with these options.
3. Use `anonymizer.Anonymize(dicomFile)` to remove sensitive attributes.
4. Save the anonymized file for further use.

```csharp
// Create a default confidentiality profile with basic anonymization rules
Aspose.Medical.Dicom.Anonymization.ConfidentialityProfile profile = ConfidentialityProfile.CreateDefault(ConfidentialityProfileOptions.CleanGraph);

// Create anonymizer using this profile
Aspose.Medical.Dicom.Anonymization.Anonymizer anonymizer = new(profile);

// Load a DICOM dataset
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("input.dcm");

// Anonymize the dataset using the profile
Aspose.Medical.Dicom.DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save result
anonymizedFile.Save("anonymized_output.dcm");
```

Confidentiality Profile options described in `DICOM` PS 3.15: https://dicom.nema.org/medical/dicom/current/output/chtml/part15/chapter_E.html#table_E.1-1
The relation between `Aspose.Medical` profiles and the ones defined in `DICOM` PS3.15:

* `BasicProfile` - Basic Profile. `XML/JSON` column name is `basic_profile`.
* `RetainSafePrivate` - Retain Safe Private Option Profile. `XML/JSON` column name is `rtn_safe_priv_opt`.
* `RetainUIDs` - Retain UIDs Option Profile. `XML/JSON` column name is `rtn_uids_opt`.
* `RetainDeviceIdent` - Retain Device Ident Option Profile. `XML/JSON` column name is `rtn_dev_id_opt`.
* `RetainInstitutionIdent` - Retain Institution Ident Option Profile. `XML/JSON` column name is `rtn_inst_id_opt`.
* `RetainPatientChars` - Retain Patient Chars Option Profile. `XML/JSON` column name is `rtn_pat_chars_opt`.
* `RetainLongFullDates` - Retain Long Full Dates Option Profile. `XML/JSON` column name is `rtn_long_full_dates_opt`.
* `RetainLongModifDates` - Retain Long Modif Dates Option Profile. `XML/JSON` column name is `rtn_long_modif_dates_opt`.
* `CleanDesc` - Clean Desc Option Profile. `XML/JSON` column name is `clean_desc_opt`.
* `CleanStructdCont` - Clean Structd Cont Option Profile. `XML/JSON` column name is `clean_struct_cont_opt`.
* `CleanGraph` - Clean Graph Option Profile. `XML/JSON` column name is `clean_graph_opt`.

There is a virtual option `All` that is the union of all the listed options.

Confidentiality actions determine how sensitive data is processed.

* `D` - Remove the data completely;
* `Z` - Replace the value with a zero-length string;
* `X` - Replace value with dummy data;
* `K` - Keep the value unchanged;
* `C` - Replace with a coded phrase;
* `U` - Replace with universally unique identifier (`UID`).

### Custom Anonymization Profiles

`Aspose.Medical` supports custom anonymization profiles that can be loaded from `JSON`, `XML`, `CSV` files.

#### Loading Custom Profiles from `CSV`

A `CSV` file can define anonymization rules for specific `DICOM` tags.

`CSV` Structure Example:

```csv
TagPattern;Action
0010,0010;Z  // Anonymize PatientName
0010,0020;D  // Remove PatientID
0020,000D;U  // Replace StudyInstanceUID
```

To load a profile from CSV file, please use the following code snippet:

```csharp
// Load confidentiality profile from a CSV file
Aspose.Medical.Dicom.Anonymization.ConfidentialityProfile profile = ConfidentialityProfile.LoadFromCsvFile("profile.csv", ConfidentialityProfileOptions.All);

// Create anonymizer with custom profile
Aspose.Medical.Dicom.Anonymization.Anonymizer anonymizer = new(profile);
```

#### Loading Custom Profiles from `JSON`

A `JSON` file can define anonymization rules for specific `DICOM` tags.

`JSON` Structure Example:

```json
[
    { "Tag": "0010,0010", "Action": "Z" },
    { "Tag": "0010,0020", "Action": "D" },
    { "Tag": "0020,000D", "Action": "U" }
]
```

To load a profile from `JSON` file, please use the following code snippet:

```csharp
// Load confidentiality profile from a JSON file
Aspose.Medical.Dicom.Anonymization.ConfidentialityProfile profile = ConfidentialityProfile.LoadFromJsonFile("profile.json", ConfidentialityProfileOptions.All);

// Create anonymizer with custom profile
Aspose.Medical.Dicom.Anonymization.Anonymizer anonymizer = new(profile);
```

#### Loading Custom Profiles from `XML`

`XML` file can define anonymization rules for specific `DICOM` tags.

`XML` Structure Example:

```xml
<root>
  <item>
    <tag>0010,0010</tag>
    <action>Z</action>
  </item>
  <item>
    <tag>0010,0020</tag>
    <action>D</action>
  </item>
</root>
```

To load a profile from `XML` file, please use the following code snippet:

```csharp
// Load confidentiality profile from an XML file
Aspose.Medical.Dicom.Anonymization.ConfidentialityProfile profile = ConfidentialityProfile.LoadFromXmlFile("profile.xml", ConfidentialityProfileOptions.All);

// Create an anonymizer with the custom profile
Aspose.Medical.Dicom.Anonymization.Anonymizer anonymizer = new(profile);
```

## Advanced Anonymization

Certain scenarios require more control over how sensitive data is altered. You can manually specify how sensitive information (like patient names and `IDs`) should be replaced.
To anonymize a `DICOM` file using custom settings for `PatientName` and `PatientID` tags, please follow the steps below:

1. Initialize a new instance of the `Aspose.Medical.Dicom.Anonymization.ConfidentialityProfile` that replaces the `PatientName` and `PatientId`.
2. Applying this profile anonymizes files while keeping certain identifiers intact.
3. Save the anonymized file for further use.

```csharp
// Load a DICOM file
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create a confidentiality profile and set custom values
Aspose.Medical.Dicom.Anonymization.ConfidentialityProfile profile = new()
{
    PatientName = "ANONYMOUS PATIENT",
    PatientId = "00000000"
};

// Create an anonymizer with this profile
Aspose.Medical.Dicom.Anonymization.Anonymizer anonymizer = new(profile);

// Anonymize the file
DicomFile anonymizedFile = anonymizer.Anonymize(dicomFile);

// Save the anonymized file
anonymizedFile.Save("custom_anonymized.dcm");
```

## In-Place Anonymization

Instead of generating a new file, you may want to modify the `DICOM` file directly.To anonymize a `DICOM` file without generating a new file, please follow the steps below:
1. Load an existing DICOM file using `Aspose.Medical.Dicom.DicomFile.Open`.
2. Create an anonymizer instance (`Aspose.Medical.Dicom.Anonymization.Anonymizer`) with default settings.
3. Use `anonymizer.AnonymizeInPlace(dicomFile)` to remove sensitive attributes.
4. Save the anonymized file for further use.


```csharp
// Load a DICOM file
Aspose.Medical.Dicom.DicomFile dicomFile = DicomFile.Open("input.dcm");

// Create anonymizer
Aspose.Medical.Dicom.Anonymization.Anonymizer anonymizer = new();

// Perform in-place anonymization
anonymizer.AnonymizeInPlace(dicomFile);

// Save the modified file
dicomFile.Save("inplace_anonymized.dcm");
```