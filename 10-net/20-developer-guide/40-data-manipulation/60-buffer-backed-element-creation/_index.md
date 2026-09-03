---
title: Low-level buffer-backed DICOM element creation
type: docs
weight: 60
url: /net/developer-guide/data-manipulation/buffer-backed-element-creation/
---

# Low-level buffer-backed DICOM element creation

The regular element constructors are the simplest choice when an application has individual values or a span of values. They validate the input and give the element its own managed storage. The buffer-backed API is intended for lower-level integrations that already own encoded bytes or that need to supply their own typed storage policy.

Concrete value element classes that implement `IBufferBackedElement<TSelf, TValue>` expose a static `Create` method. It accepts one of two storage representations:

| Representation | What the element retains | Suitable when |
|---|---|---|
| `IByteBuffer` | Encoded DICOM value-field bytes | The bytes already exist and typed values might never be requested |
| `ITypedBuffer<T>` | Materialized CLR values | The application already owns typed data or needs custom mutation and storage behavior |

The selected buffer instance is retained without copying its contents during element creation. This can remove an initial copy, but it also makes buffer lifetime, aliasing, encoding, and mutation the application's responsibility.

## Selecting a buffer representation

Call `Create` on the concrete element type and set exactly one tuple component:

```csharp
internal static class ByteBufferSelectionExample
{
    internal static Aspose.Medical.Dicom.Elements.SignedShort Create(
        Aspose.Medical.Common.Buffers.IByteBuffer byteBuffer)
        => Aspose.Medical.Dicom.Elements.SignedShort.Create(
            Aspose.Medical.Dicom.Tags.Tag.PixelPaddingValue,
            (ByteBuffer: byteBuffer, TypedBuffer: null));
}
```

If both components are `null`, `Create` throws `ArgumentException`. If both are non-null, `ByteBuffer` takes precedence. Supplying exactly one component makes the intended representation explicit.

This API applies to the following buffer-backed value elements:

- Binary and numeric values: `AttributeTag`, `FloatingPointDouble`, `FloatingPointSingle`, `OtherByte`, `OtherDouble`, `OtherFloat`, `OtherLong`, `OtherVeryLong`, `OtherWord`, `SignedLong`, `SignedShort`, `SignedVeryLong`, `Unknown`, `UnsignedLong`, `UnsignedShort`, and `UnsignedVeryLong`.
- Text-backed values: `AgeString`, `ApplicationEntity`, `CodeString`, `Date`, `DateTime`, `DecimalString`, `IntegerString`, `LongString`, `LongText`, `PersonName`, `ShortString`, `ShortText`, `Time`, `UniqueIdentifier`, `UniversalResource`, `UnlimitedCharacters`, and `UnlimitedText`.

`Sequence` and `FragmentSequence` use their own construction APIs. For text-backed elements, including date and numeric-text VRs, `TValue` is `string`; for example, `DecimalString.Create` accepts `ITypedBuffer<string>`, not `ITypedBuffer<decimal>`.

## Retaining encoded bytes with `IByteBuffer`

`IByteBuffer.Span` must expose only the encoded value field. It must not include the tag, Value Representation (VR), or encoded length. The element retains the buffer and materializes typed values only when an operation requires them. Accessing `Data`, `Get`, `GetValues`, or `Count`, and modifying the element, requires typed values and therefore materializes encoded storage. A compatible binary DICOM write can copy encoded bytes directly without first creating typed storage.

For multi-byte primitive VRs, the public `Create` methods interpret the supplied bytes in the current platform's byte order. Prepare those bytes according to `BitConverter.IsLittleEndian`. Byte-oriented VRs do not have an endianness concern. Text element factories use the default DICOM text encoding; the current low-level factory does not accept a custom character-set context.

The following buffer wraps an existing `Memory<byte>` slice. Its `Span` getter does no conversion and allocates nothing:

```csharp
internal sealed class MemoryByteBuffer(System.Memory<byte> memory) :
    Aspose.Medical.Common.Buffers.IByteBuffer
{
    public System.Span<byte> Span => memory.Span;
}

internal static class BufferBackedElementExamples
{
    internal static Aspose.Medical.Dicom.Elements.SignedShort CreateSignedShort(
        short value)
    {
        System.Memory<byte> encodedValue = new byte[sizeof(short)];

        if (System.BitConverter.IsLittleEndian)
        {
            System.Buffers.Binary.BinaryPrimitives.WriteInt16LittleEndian(
                encodedValue.Span,
                value);
        }
        else
        {
            System.Buffers.Binary.BinaryPrimitives.WriteInt16BigEndian(
                encodedValue.Span,
                value);
        }

        MemoryByteBuffer byteBuffer = new(encodedValue);

        return Aspose.Medical.Dicom.Elements.SignedShort.Create(
            Aspose.Medical.Dicom.Tags.Tag.PixelPaddingValue,
            (ByteBuffer: byteBuffer, TypedBuffer: null));
    }
}
```

The `Memory<byte>` can refer to an array, an `IMemoryOwner<byte>`, a custom `MemoryManager<byte>`, memory-mapped data exposed through a suitable manager, or another stable .NET memory source. Preserve the exact logical slice: extra capacity in `Span` is treated as part of the DICOM value field.

### Keep `Span` access repeatable and inexpensive

Typed materialization is published with an optimistic lock-free update. If multiple threads request typed data concurrently, conversion can run more than once even though only one materialized state is retained. Some codecs also access `Span` more than once during one conversion.

Consequently, `Span` should return already prepared, stable memory. Do not make the getter consume a stream, advance a cursor, rent a new buffer, decompress data, perform remote I/O, or produce side effects. Perform expensive work once before creating the element, or cache its result independently of the getter. The same bytes must remain available for every access until materialization has completed.

### Use pooled byte memory safely

`MemoryPool<byte>` can avoid allocating a temporary byte array when a producer can write directly into rented memory. The owner must remain alive while the element can still access the encoded buffer. If the application wants to return the memory immediately, force materialization before disposal and ensure no concurrent operation is still reading the buffer:

```csharp
internal static class PooledByteBufferExample
{
    internal static Aspose.Medical.Dicom.Elements.SignedShort CreateFromPooledBytes(
        short value)
    {
        using System.Buffers.IMemoryOwner<byte> owner =
            System.Buffers.MemoryPool<byte>.Shared.Rent(sizeof(short));

        System.Memory<byte> encodedValue = owner.Memory[..sizeof(short)];

        if (System.BitConverter.IsLittleEndian)
        {
            System.Buffers.Binary.BinaryPrimitives.WriteInt16LittleEndian(
                encodedValue.Span,
                value);
        }
        else
        {
            System.Buffers.Binary.BinaryPrimitives.WriteInt16BigEndian(
                encodedValue.Span,
                value);
        }

        MemoryByteBuffer byteBuffer = new(encodedValue);
        Aspose.Medical.Dicom.Elements.SignedShort element =
            Aspose.Medical.Dicom.Elements.SignedShort.Create(
                Aspose.Medical.Dicom.Tags.Tag.PixelPaddingValue,
                (ByteBuffer: byteBuffer, TypedBuffer: null));

        _ = element.Data;
        return element;
    }
}
```

Without the `Data` access, materialization remains deferred and disposing `owner` would invalidate the retained buffer. For long-lived deferred elements, let a buffer object own the `IMemoryOwner<byte>` and dispose it only after the element is no longer used. Aspose.Medical does not dispose application-provided buffers.

## Retaining materialized values with `ITypedBuffer<T>`

An `ITypedBuffer<T>` supplies the element's current typed contents and implements structural mutation:

- `Length` reports the current logical value count.
- `Get` returns the current contents as mutable `Memory<T>`.
- `Add`, `AddRange`, `Insert`, and `RemoveAt` implement the corresponding element mutations.

The memory returned by `Get` is exposed as the element's live data view. It must contain exactly `Length` logical values in one contiguous region. The underlying array or rented block may be larger, but `Get` must return a slice that excludes unused capacity. This makes an oversized `ArrayPool<T>` or `MemoryPool<T>` allocation usable without exposing capacity as DICOM values.

Keep `Length` and `Get` inexpensive and allocation-free because data access, validation, size calculation, and serialization can query them repeatedly. Do not perform conversion, I/O, decompression, or other expensive work in `Get`.

The following implementation retains an existing `Memory<T>` slice without an initial copy. Reads allocate nothing. Structural mutations publish a new exact-length managed array because the retained memory has fixed capacity. The lock coordinates those structural mutations; it does not prevent callers from modifying values through memory previously returned by `Get` or through the element's `Data` property.

```csharp
internal sealed class MemoryTypedBuffer<T> :
    Aspose.Medical.Common.Buffers.ITypedBuffer<T>
{
    public int Length => System.Threading.Volatile.Read(ref _state).Values.Length;

    public System.Memory<T> Get() =>
        System.Threading.Volatile.Read(ref _state).Values;

    public void Add(T value)
    {
        lock (_gate)
        {
            System.Span<T> current = _state.Values.Span;
            T[] updated = new T[current.Length + 1];
            current.CopyTo(updated);
            updated[^1] = value;
            Publish(updated);
        }
    }

    public void AddRange(System.ReadOnlySpan<T> values)
    {
        if (values.IsEmpty)
            return;

        lock (_gate)
        {
            System.Span<T> current = _state.Values.Span;
            T[] updated = new T[current.Length + values.Length];
            current.CopyTo(updated);
            values.CopyTo(updated.AsSpan(current.Length));
            Publish(updated);
        }
    }

    public void Insert(int index, T value)
    {
        lock (_gate)
        {
            System.Span<T> current = _state.Values.Span;
            if ((uint)index > (uint)current.Length)
                throw new System.ArgumentOutOfRangeException(nameof(index));

            T[] updated = new T[current.Length + 1];
            current[..index].CopyTo(updated);
            updated[index] = value;
            current[index..].CopyTo(updated.AsSpan(index + 1));
            Publish(updated);
        }
    }

    public void RemoveAt(int index)
    {
        lock (_gate)
        {
            System.Span<T> current = _state.Values.Span;
            if ((uint)index >= (uint)current.Length)
                throw new System.ArgumentOutOfRangeException(nameof(index));

            T[] updated = new T[current.Length - 1];
            current[..index].CopyTo(updated);
            current[(index + 1)..].CopyTo(updated.AsSpan(index));
            Publish(updated);
        }
    }

    internal MemoryTypedBuffer(System.Memory<T> values)
        => _state = new State(values);

    private readonly object _gate = new();
    private State _state;

    private void Publish(T[] values) =>
        System.Threading.Volatile.Write(ref _state, new State(values));

    private sealed record State(System.Memory<T> Values);
}
```

Use the typed buffer as the second tuple component:

```csharp
internal static class TypedBufferExample
{
    internal static Aspose.Medical.Dicom.Dataset CreateDataset()
    {
        uint[] initialValues = [10u, 20u, 30u];
        MemoryTypedBuffer<uint> typedBuffer = new(initialValues.AsMemory());

        Aspose.Medical.Dicom.Elements.UnsignedLong element =
            Aspose.Medical.Dicom.Elements.UnsignedLong.Create(
                Aspose.Medical.Dicom.Tags.Tag.DimensionIndexValues,
                (ByteBuffer: null, TypedBuffer: typedBuffer));

        element.AddRange([40u, 50u]);

        Aspose.Medical.Dicom.Dataset dataset = new();
        dataset.Add(element);
        return dataset;
    }
}
```

Do not modify `initialValues` after passing it to the buffer unless shared mutation is intentional. `Memory<T>` is a view rather than an owner, so the memory's backing storage must remain valid while the element or any memory obtained from it can still be used. If the memory comes from `IMemoryOwner<T>`, the application must keep that owner alive for the required lifetime; Aspose.Medical does not dispose application-provided typed buffers or their owners.

The element's `Data` property returns the current mutable memory view. Use the element indexer for individual values and `Data.Span` for allocation-free synchronous bulk access:

```csharp
internal static class TypedDataAccessExample
{
    internal static uint[] UpdateAndCopy(
        Aspose.Medical.Dicom.Elements.UnsignedLong element)
    {
        element[0] = 100u;

        System.Span<uint> values = element.Data.Span;
        values[1] = 200u;

        return element.CopyDataToArray();
    }
}
```

Writes through the indexer or `Data.Span` change existing values but bypass structural mutation methods and DICOM value validation. `CopyDataToArray` explicitly allocates an independent array; modifying that array does not modify the element. `Memory<T>` does not implement `IEnumerable<T>`, so use the indexer or `Data.Span` for direct processing instead of converting to an array merely to enumerate it.

Calling the element's `Replace` method installs a new internal managed-array buffer, so the custom typed buffer no longer backs the element. Likewise, `Add`, `AddRange`, `Insert`, or `RemoveAt` may replace a custom buffer's current memory. Do not assume that previously obtained `Memory<T>` remains the element's current view after structural mutation; it may still reference the previous backing storage.

## Allocation and ownership checklist

- Prefer regular constructors when the application does not need retained custom storage; they have simpler ownership semantics.
- Prefer `IByteBuffer` when encoded bytes already exist and the element can often remain encoded, such as a read-modify-write path that does not inspect the value.
- Expose an exact memory slice and keep `IByteBuffer.Span` repeatable, side-effect-free, and allocation-free.
- Keep an encoded buffer alive until materialization completes or until the element is no longer used. Do not return pooled memory while concurrent access is possible.
- Prefer `ITypedBuffer<T>` when typed memory already exists or a custom mutation policy is required. Make `Length` and `Get` allocation-free in the common path.
- Do not wrap `List<T>` by calling `ToArray` from every `Get`, and do not expose unused `ArrayPool<T>` capacity as values.
- Batch structural changes with `AddRange` where practical. Copy-on-write buffers otherwise allocate a replacement array for each individual structural mutation.
- Treat retained encoded memory and memory returned by `ITypedBuffer<T>.Get` as shared state. Coordinate writes explicitly and do not assume that buffer-backed elements are fully thread-safe.
