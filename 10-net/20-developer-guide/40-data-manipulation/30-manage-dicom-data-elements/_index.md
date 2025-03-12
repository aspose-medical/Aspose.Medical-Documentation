---
title: Working with DICOM Data Elements Using Aspose.Medical
type: docs
weight: 30
url: /net/developer-guide/data-manipulation/manage-dicom-data-elements/
---

# Working with `DICOM` Data Elements Using `Aspose.Medical`

This documentation provides a guide on managing DICOM data elements using the `Aspose.Medical.Dicom.Elements.IElement` API. The focus is on adding, updating, removing, and replacing data for DICOM data elements.

**Note**: This guide uses `FloatingPointDouble` (an implementation of `IElement` that works with double-precision floating-point values) as an example. `Aspose.Medical` provides the following classes that matches with the VR (Value Representation) defined in the `DICOM` specification. All these classes can be found in `Aspose.Medical.Dicom.Elements` namespace.

- `AgeString` - [Age String (AS) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=AS-,Age%20String,-A%20string%20of)
- `ApplicationEntity` - [Application Entity (AE) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=AE-,Application%20Entity,-A%20string%20of)
- `AttributeTag` - [Attribute Tag (AT) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=AT-,Attribute%20Tag,-Ordered%20pair%20of)
- `CodeString` - [Code String (CS) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=CS-,Code%20String,-A%20string%20of)
- `Date` - [Date (DA) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=DA-,Date,-A%20string%20of)
- `DateTime` - [Date Time (DT) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=DT-,Date%20Time,-A%20concatenated%20date)
- `DecimalString` - [Decimal String (DS) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=DS-,Decimal%20String,-A%20string%20of)
- `FloatingPointDouble` - [Floating Point Double (FD) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=FD-,Floating%20Point%20Double,-Double%20precision%20binary)
- `FloatingPointSingle` - [Floating Point Single (FL) VR]()
- `IntegerString` - [Integer String (IS) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=IS-,Integer%20String,-A%20string%20of)
- `LongString` - [Long String (LO) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=LO-,Long%20String,-A%20character%20string)
- `LongText` - [Long Text (LT) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=LT-,Long%20Text,-A%20character%20string)
- `OtherByte` - [Other Byte (OB) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=OB-,Other%20Byte,-An%20octet%2Dstream)
- `OtherDouble` - [Other Double (OD) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=OD-,Other%20Double,-A%20stream%20of)
- `OtherFloat` - [Other Float (OF) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=OF-,Other%20Float,-A%20stream%20of)
- `OtherLong` - [Other Long (OL) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=OL-,Other%20Long,-A%20stream%20of)
- `OtherVeryLong` - [Other Very Long (OV) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=Transfer%20Syntax%20definition-,OV,-Other%2064%2Dbit)
- `OtherWord` - [Other Word (OW) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=OW-,Other%20Word,-A%20stream%20of)
- `PersonName` - [Person Name (PN) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=PN-,Person%20Name,-A%20character%20string)
- `Sequence` - [Sequence of Items (SQ) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=SQ-,Sequence%20of%20Items,-Value%20is%20a)
- `ShortString` - [Short String (SH) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=SH-,Short%20String,-A%20character%20string)
- `ShortText` - [Short Text (ST) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=ST-,Short%20Text,-A%20character%20string)
- `SignedLong` - [Signed Long (SL) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=SL-,Signed%20Long,-Signed%20binary%20integer)
- `SignedShort` - [Signed Short (SS) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=SS-,Signed%20Short,-Signed%20binary%20integer)
- `SignedVeryLong` - [Signed Very Long (SV) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=bit%20Very%20Long-,Signed%20binary%20integer%2064%20bits%20long,-.%20Represents%20an%20integer)
- `Time` - [Time (TM) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=TM-,Time,-A%20string%20of)
- `UniqueIdentifier` - [Unique Identifier (UI) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=UI-,Unique%20Identifier,-(UID))
- `UniversalResource` - [Universal Resource Identifier or Universal Resource Locator (UR) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=DICOM%20Value%20Representations-,UR,-Universal%20Resource%20Identifier)
- `Unknown` - [Unknown (UN) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=UN-,Unknown,-An%20octet%2Dstream)
- `UnlimitedCharacters` - [Unlimited Characters (UC) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=Unlimited%20Characters)
- `UnlimitedText` - [Unlimited Text (UT) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=UT-,Unlimited%20Text,-A%20character%20string)
- `UnsignedLong` - [Unsigned Long (UL) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=UL-,Unsigned%20Long,-Unsigned%20binary%20integer)
- `UnsignedShort` - [Unsigned Short (US) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=US-,Unsigned%20Short,-Unsigned%20binary%20integer)
- `UnsignedVeryLong` - [Unsigned Very Long (UV) VR](https://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_6.2.html#:~:text=See%20Note%202-,UV,-Unsigned%2064%2Dbit)


## Retrieving Data from a `DICOM` Element

Retrieving data from an element can be done using several approaches provided by the `API`.

- `Get<T>(int index)`: Retrieves a specific value by position.
- `Get<T>(Index index)`: Retrieves a specific value by position.
- `GetValues<T>()`: Retrieves all stored values.
- `GetOrDefault<T>(int index)`: Retrieves a value at a specified index or returns a default value if the index is out of range.

The following snippet demonstrates how the listed methods work.

```csharp
// Create a DICOM Tag for the element
Aspose.Medical.Dicom.Tags.Tag tag = Tag.TableOfYBreakPoints;

// Initialize FloatingPointDouble with a value
Aspose.Medical.Dicom.Elements.FloatingPointDouble element = new(tag, [50.1, 100.2, 150.3]);

// Retrieve a value by numeric index
double firstValue = element.Get<float>(0);
Console.WriteLine("First Value: " + firstValue); // 50.1

// Retrieve a value by index
double lastValue = element.Get<double>(^1);
Console.WriteLine("Last Value: " + lastValue); // 150.3

// Retrieve all values
System.Span<double> allValues = element.GetValues<double>();
foreach (double val in allValues)
{
    Console.WriteLine("Value: " + val);
}

// Retrieve a value at a specific position safely
double defaultValue = element.GetOrDefault<double>(4);
Console.WriteLine("Value (Safe Retrieval): " + defaultValue); // default(double), e.g., 0
```

## Manage Data of a `DICOM` Element

Although Aspose.Medical.Dicom.Elements.IElement does not provide methods to modify element data, such methods are typically available at the element type level. This approach helps prevent common mistakes related to data manipulation.

### Adding Data to a `DICOM` Element

You can use the `Add` or `AddRange` methods of `FloatingPointDouble` to append values to a `DICOM` element.

To add a single value to an element:

```csharp
// Create a DICOM Tag for the element
Aspose.Medical.Dicom.Tags.Tag tag = Tag.TableOfYBreakPoints;

// Initialize FloatingPointDouble with a value
Aspose.Medical.Dicom.Elements.FloatingPointDouble element = new(tag, []);

// The element now contains one floating-point value: 42.5
element.Add(42.5);
```

To add multiple values to an element at once:

```csharp
// Create a DICOM Tag for the element
Aspose.Medical.Dicom.Tags.Tag tag = Tag.TableOfYBreakPoints;

// Initialize FloatingPointDouble with a value
Aspose.Medical.Dicom.Elements.FloatingPointDouble element = new(tag, []);

double[] values = [10.1, 20.2, 30.3];
// The element now contains values: [10.1, 20.2, 30.3]
element.AddRange(values);
```

### Inserting Data into a `DICOM` Element

You can insert a value at a specific index using the `Insert` method.

```csharp
// Create a DICOM Tag for the element
Aspose.Medical.Dicom.Tags.Tag tag = Tag.TableOfYBreakPoints;

// Initialize FloatingPointDouble with a value
Aspose.Medical.Dicom.Elements.FloatingPointDouble element = new(tag, [1.1, 2.2, 4.4]);

// The element now contains values: [1.1, 2.2, 3.3, 4.4]
element.Insert(2, 3.3);
```

### Removing Data from a `DICOM` Element

You can remove values from an element using `Remove` or `RemoveAt`.

The `Remove` method deletes the first occurrence of a specific value.

```csharp
// Create a DICOM Tag for the element
Aspose.Medical.Dicom.Tags.Tag tag = Tag.TableOfYBreakPoints;

// Initialize FloatingPointDouble with a value
Aspose.Medical.Dicom.Elements.FloatingPointDouble element = new(tag, [5.5, 6.6, 7.7, 6.6]);

// The element now contains values: [5.5, 7.7, 6.6]
element.Remove(6.6);
```

The `RemoveAt` method removes the value at a specified index.

```csharp
// Create a DICOM Tag for the element
Aspose.Medical.Dicom.Tags.Tag tag = Tag.TableOfYBreakPoints;

// Initialize FloatingPointDouble with a value
Aspose.Medical.Dicom.Elements.FloatingPointDouble element = new(tag, [11.1, 22.2, 33.3]);

// The element now contains values: [11.1, 33.3]
element.RemoveAt(1);
```

## Replacing Data in a `DICOM` Element

The `Replace` method allows resetting the element’s values with a new collection.

```csharp
// Create a DICOM Tag for the element
Aspose.Medical.Dicom.Tags.Tag tag = Tag.TableOfYBreakPoints;

// Initialize FloatingPointDouble with a value
Aspose.Medical.Dicom.Elements.FloatingPointDouble element = new(tag, [100.1, 200.2, 300.3]);

// The element now contains values: [400.4, 500.5]
element.Replace([400.4, 500.5 ]);
```
