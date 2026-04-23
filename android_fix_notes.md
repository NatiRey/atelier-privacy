# Ceramistic Kotlin type mismatch fixes

I couldn't directly edit `C:/Users/natir/AndroidStudioProjects/Ceramistic/...` from this container, so here is the exact patch to apply in `AddStockMainForm.kt`.

## 1) `Function1<Proveedor, Unit>` -> `Function1<Proveedor?, Unit>`

At (around) line 211, your callback now expects a nullable provider. Update your lambda parameter to `Proveedor?` and handle null safely.

```kotlin
onProveedorSelected = { proveedor: Proveedor? ->
    proveedor?.let { selected ->
        onProveedorSelected(selected)
    }
}
```

If you pass directly into a ViewModel function, same pattern:

```kotlin
onProveedorSelected = { proveedor: Proveedor? ->
    proveedor?.let(viewModel::onProveedorSelected)
}
```

## 2) `Function1<List<Int>, Unit>` -> `Function1<Set<Int>, Unit>`

At (around) line 247, the component now emits a `Set<Int>`. If your downstream function still expects `List<Int>`, convert it:

```kotlin
onSelectedWarehousesChange = { ids: Set<Int> ->
    onSelectedWarehousesChange(ids.toList())
}
```

Or with ViewModel:

```kotlin
onSelectedWarehousesChange = { ids: Set<Int> ->
    viewModel.onSelectedWarehousesChange(ids.toList())
}
```

If you can change the downstream signature, make it accept `Set<Int>` directly to avoid conversions.
