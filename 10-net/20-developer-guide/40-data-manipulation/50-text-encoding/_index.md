---
title: Custom Text Encoding
type: docs
weight: 50
url: /net/developer-guide/data-manipulation/text-encoding/
---

# Custom Text Encoding

This documentation outlines how to integrate a custom text encoding/decoding mechanism within `Aspose.Medical` `.NET`'s Encoding infrastructure. It enables developers to define and register custom text encodings that can be used when handling DICOM attributes with character sets that are not supported natively or need specialized handling.

## Integration

The integration is composed of:

1. Custom Encoding
2. Custom EncodingProvider
3. Registering the EncodingProvider using `Aspose.Medical.Common.Text.DicomEncoding.Current.RegisterProvider` method.
4. Registering the Encoding using `Aspose.Medical.Common.Text.DicomEncoding.Current.RegisterEncoding` method.

Custom `Encoding` should override/implement:
1. `GetBytes` and all its overloads.
2. `GetChars` and all its overloads.
3. `GetString` and all its overloads.
4. `GetMaxByteCount` and all its overloads.
5. `GetByteCount` and all its overloads.
6. `GetCharCount` and all its overloads.

Custom `EncodingProvider` should override/implement:
1. `Encoding? GetEncoding(string name)`
2. `Encoding? GetEncoding(int codepage)`
3. `Encoding? GetEncoding(int codepage, EncoderFallback encoderFallback, DecoderFallback decoderFallback)`
4. `Encoding? GetEncoding(string name, EncoderFallback encoderFallback, DecoderFallback decoderFallback)`

Example:

```csharp

public sealed class Iso_8859_6_Encoding : Encoding {
  // implementation skipped
}

public sealed class CustomEncodingProvider : EncodingProvider {
  public override Encoding? GetEncoding(string name)
    => string.Equals(name, "iso-8859-6", StringComparison.OrdinalIgnoreCase) ||
       string.Equals(name, Gb18030.WebName, StringComparison.OrdinalIgnoreCase)
           ? new Iso_8859_6_Encoding()
           : null;

  // implementation skipped
}

CustomEncodingProvider provider = new();
Aspose.Medical.Common.Text.DicomEncoding.Current.RegisterProvider(provider);
Aspose.Medical.Common.Text.DicomEncoding.Current.RegisterEncoding("ISO 2022 IR 127", "iso-8859-6");

// now Aspose.Medical will use the custom encoding to encode/decode text elements

Aspose.Medical.Dicom.Dataset dataset = [];
dataset.AddOrUpdate(Aspose.Medical.Dicom.Tags.Tag.SpecificCharacterSet, "ISO 2022 IR 127");
dataset.AddOrUpdate(Aspose.Medical.Dicom.Tags.Tag.PatientName, patientName);

Aspose.Medical.Dicom.DicomFile initial = new(dataset);
using System.IO.MemoryStream stream = new();

// Save uses custom encoding to encode PatientName
initial.Save(stream);

stream.Seek(0, System.IO.SeekOrigin.Begin);

Aspose.Medical.Dicom.DicomFile saved = Aspose.Medical.Dicom.DicomFile.Open(stream);

// PatientName will be decoded using custom encoding
patientName = saved.Dataset.GetValue<string>(Aspose.Medical.Dicom.Tags.Tag.PatientName, 0);
```
