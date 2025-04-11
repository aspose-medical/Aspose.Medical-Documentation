---
title: Adding Private Tags to Tag Dictionary Using Aspose.Medical
type: docs
weight: 40
url: /net/developer-guide/data-manipulation/adding-private-tags-to-tag-dictionary/
---

# Adding Private `Tags` to `Tag Dictionary` Using `Aspose.Medical`

In order to ensure that tag descriptions and value representations are shown correctly, it is necessary to load a private tags file for the purpose of adding private tags to the default `Tag Dictionary` in `Aspose.Medical`.

Please use the following code snippet to load private tags from XML file:

```csharp
// Create a DICOM Tag for the element
const string tagsXml = "path\to\custom-tag-dictionary.xml";

Aspose.Medical.Dicom.Tags.TagDictionary.Default.Load(tagsXml);
```

`XML` file must have the following structure:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<dictionaries>
  <dictionary creator="CUSTOM PRIVATE CREATOR">
    <tag group="3011" element="xx00" vr="SL" vm="1">Private tag # 1-1 (description)</tag>
    <tag group="3011" element="xx01" vr="FD" vm="2">Private tag # 1-2 (description)</tag>
    <tag group="3011" element="xx02" vr="DS" vm="1-n">Private tag #N (description)</tag>
  </dictionary>
  <dictionary creator="CUSTOM PRIVATE CREATOR #2">
    <tag group="4001" element="xx00" vr="FD" vm="2-7">Private tag # 2-1</tag>
  </dictionary>
</dictionaries>
```
