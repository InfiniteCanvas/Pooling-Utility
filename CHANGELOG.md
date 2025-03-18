# CHANGELOG

## [1.0.0] - 2025-03-18

- Relocated `Pooling Utility` from `Unity Utilities` to its own package

### Added

Source generator that adds these methods to classes tagged with `[Pooled]`
- `static T Get()` - Get an instance from the pool
- `static PooledObject<T> Get(out T instance)` - Get an instance with IDisposable support
- `static void Release(T instance)` - Return an instance to the pool
- `static void Clear()` - Clear the pool
