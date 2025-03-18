# Unity Pooled Attribute

A source generator package that automatically adds object pooling functionality to any class marked with the `[Pooled]` attribute.

## Features

- Automatic object pool generation through source generation
- Support for custom creation, get, return, and destroy actions
- Configurable pool capacity and max size
- Compatible with both MonoBehaviour and regular C# classes (though MonoBehaviours need special considerations for creation and disposal)
- Supports nested classes with proper accessibility modifiers
- Zero reflection overhead at runtime
- Using Unity's ObjectPool<T> implementation 

## Tested Unity Versions

- Unity 6000.1

## Usage

### Basic Usage
```csharp
using UnityEngine;
using UnityPoolingUtility;

[Pooled]
public partial class Message
{
}

// Usage:
Message message = Message.Get();
Message.Release(message);
```

### Custom Pool Configuration
```csharp
[Pooled(
    //using the utility class to load an addressable
    createAction: "() => Object.Instantiate<Bullet>(AddressablesLoader.GetAsset<Bullet>(\"Bullet\"))", 
    getAction: "item => item.gameObject.SetActive(true)",
    returnAction: "item => item.gameObject.SetActive(false)",
    destroyAction: "item => Object.Destroy(item.gameObject)",
    defaultCapacity: 20,
    maxSize: 100
)]
public partial class Bullet : MonoBehaviour
{
}
```

### Using with Nested Classes
```csharp
public class Weapon
{
    [Pooled]
    internal partial class Bullet : MonoBehaviour
    {
    }
}
```
While supported (and tested with 2 levels of nesting), usage is discouraged.

## API Reference

### PooledAttribute
```csharp
public PooledAttribute(
    string createAction = "",        // Custom instantiation logic
    string getAction = "",          // Action when retrieving from pool
    string returnAction = "",       // Action when returning to pool
    string destroyAction = "",      // Action when destroying pooled object
    int defaultCapacity = 10,       // Initial pool capacity
    int maxSize = 100              // Maximum pool size
)
```

### Generated Methods
Each marked class gets these static methods:
- `static T Get()` - Get an instance from the pool
- `static PooledObject<T> Get(out T instance)` - Get an instance with IDisposable support
- `static void Release(T instance)` - Return an instance to the pool
- `static void Clear()` - Clear the pool

## Performance Considerations

- Zero reflection overhead at runtime
- Compile-time code generation
- Safe pool implementation using Unity's built-in solution

## Known Limitations

- Classes must be marked as `partial` and at least `internal`
- Source generation (with this source generator) requires Unity 6+
- Custom actions must be valid C# expressions as strings
