---
title: DICOM Serialization
type: docs
weight: 90
url: /net/developer-guide/90-dicom-serialization/
---


# `DICOM` Serialization/Deserialization

`Aspose.Medical` provides robust and standards-compliant support for converting DICOM datasets to and from JSON format, adhering to the guidelines set forth by the DICOM standard (specifically outlined in PS3.18 - the DICOM Web Services standard). JSON-based representation of DICOM data is crucial for modern applications that interact with DICOM content over RESTful web services and in environments that prefer human-readable or easily parsable data formats such as JSON.

The features offered by this module enable:

- Serialization: Transforming DICOM datasets into JSON that conforms to DICOMweb specifications, making it suitable for use in REST APIs, web viewers, and cloud-based storage systems.
- Deserialization: Reading and parsing standard-compliant DICOM JSON back into native DICOM object models, allowing seamless integration with existing imaging pipelines or medical device software.

This part of the library is optimized for correctness, flexibility, and extensibility. It ensures accurate preservation of data types, tags, Value Representations (VRs), and nested sequences and supports customization for handling private tags, bulk data URIs, and other DICOM-specific encoding rules.

The following subsections will provide additional information.

- [JSON Serialization/Deserialization]({{< ref "/10-net/20-developer-guide/90-dicom-serialization/10-dicom-json-serialization" >}} "JSON")