---
title: Licensing
type: docs
weight: 40
url: /net/getting-started/licensing/
---

# Licensing

## Evaluate Aspose.Medical

You can download an evaluation version of Aspose.Medical for NET from its NuGet download page. The evaluation version provides the same functionalities as the licensed version of the product. The evaluation package is the same as the purchased package. The evaluation version simply becomes licensed after you add a few lines of code to it (to apply the license).

Once you are happy with your evaluation of Aspose.Medical, you can purchase a license.

Every Aspose license comes with one-year subscription for free upgrades to new versions or fixes released within the subscription period. Users with licensed products or even evaluation versions get free and unlimited technical support.

### Evaluation version limitations

* While Aspose.Medical evaluation version (without a license specified) provides full product functionality, it inserts an evaluation watermark at the rendered images and adds a license message during save operations.
* Some tags can't be anonymized using the evaluation version.
* `DICOM` transcoding is not available with the evaluation version.

## Licensing in Aspose.Medical

* An evaluation version becomes licensed after you purchase a license and add a couple of lines of code to it (to apply the license).
* The license is a plain-text XML file that contains details such as the product name, number of developers it is licensed to, subscription expiry date, and so on.
* The license file is digitally signed, so you must not modify the file. Even an inadvertent addition of an extra line break to the contents of the file will invalidate it.
* Aspose.Medical for .NET typically tries to find the license in these locations:
  * An explicit path
  * The folder containing the dll of the component (included in Aspose.Medical)
  * The folder containing the assembly that called the dll of the component (included in Aspose.Medical)
  * The folder containing the entry assembly (your .exe)
  * An embedded resource in the assembly that called the dll of the component (included in Aspose.Medical).
* To avoid the limitations associated with the evaluation version, you need to set a license before using Aspose.Medical. You only have to set a license once per application or process.

## Applying a License

A license can be loaded from a `file`, `stream`, or `embedded resource`.

### File

The easiest method of setting a license requires you to place the license file in the same folder containing the component’s DLL (included in Aspose.Medical) and specify only the file name without its path.

This C# code shows you how to set a license file:

```csharp
// Instantiates the License class
Aspose.Medical.License license = new ();

// Sets the license file path
license.SetLicense("Aspose.Medical.lic");
```

**Note**:
- If you place the license file in a different directory, when you call the `SetLicense` method, the license file name at the end of the specified explicit must be the same as your license file.
- For example, you can change the license file name to `Aspose.Medical.lic.xml`. Then, in your code, you have to pass the path to the file (ending with `Aspose.Medical.lic.xml`) to the SetLicense method.

### Stream

You can load a license from a stream. This C# code shows you how to apply a license from a stream:

```csharp
// Instantiates the License class
Aspose.Medical.License license = new ();

// Sets the license file path
license.SetLicense(myStream);
```

### Embedded Resource

You can package the license with your application (to avoid losing it) by adding the license as an embedded resource into one of the assemblies that call the component’s DLL (included in `Aspose.License`).

This is how you add a license file as an embedded resource:

1. In Visual Studio, add the license (.lic) file to the project this way: Go through **File > Add Existing Item > Add**.
2. Select the file in the **Solution Explorer**.
3. On the **Properties** window, set the **Build Action** to **Embedded Resource**.
4. To access the license embedded in the assembly, add the license file as an embedded resource to the project, and then pass the license file name to the `SetLicense` method

The `License` class automatically finds the license file in the embedded resources. You do not need to call the `GetExecutingAssembly` and `GetManifestResourceStream` methods of the `System.Reflection.Assembly` class in the Microsoft .NET Framework.

This C# code shows you how to set a license as an embedded resource:

```csharp
// Instantiates the License class
Aspose.Medical.License license = new ();

// Passes the license file name embedded in the assembly
license.SetLicense("Aspose.Medical.lic");
```

**Note**: The `license.SetLicense` method is **not thread-safe**. If this method has to be called simultaneously from many threads, you may want to use synchronization primitives (like a lock) to avoid issues.
