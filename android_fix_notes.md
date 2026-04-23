# Ceramistic Android dark mode onboarding images

Como agregaste las imágenes `onboarding_welcome_night` y `pizarra_tacita_night`, la forma correcta en Android es usar calificadores de recursos `-night` para que el sistema haga el cambio automático al entrar en modo oscuro.

## 1) Estructura de carpetas recomendada

Ubicá las imágenes así (mismo nombre base para light/dark):

```text
app/src/main/res/drawable/onboarding_welcome.png
app/src/main/res/drawable/pizarra_tacita.png

app/src/main/res/drawable-night/onboarding_welcome.png
app/src/main/res/drawable-night/pizarra_tacita.png
```

> Importante: En `drawable-night` **no** uses `_night` en el nombre del archivo. El selector lo hace la carpeta, no el nombre.

## 2) Referencias en las pantallas de bienvenida

En XML o Compose, seguí usando solo el nombre base:

- `onboarding_welcome`
- `pizarra_tacita`

### Si usás XML

```xml
android:src="@drawable/onboarding_welcome"
```

```xml
android:src="@drawable/pizarra_tacita"
```

### Si usás Jetpack Compose

```kotlin
Image(
    painter = painterResource(R.drawable.onboarding_welcome),
    contentDescription = null
)
```

```kotlin
Image(
    painter = painterResource(R.drawable.pizarra_tacita),
    contentDescription = null
)
```

Con eso, Android va a cargar automáticamente la variante de `drawable-night` cuando el dispositivo esté en modo oscuro.

## 3) Si querés conservar los nombres que ya creaste

Si preferís mantener `onboarding_welcome_night` y `pizarra_tacita_night`, entonces tenés que hacer la selección manual en código (no recomendado frente a `drawable-night`).

---

# Fixes para errores de tipos en `AddStockMainForm.kt`

Estos dos errores son de **nulabilidad** y de **tipo de colección**.

## Error 1

```text
actual type is 'Function1<Proveedor, Unit>', but 'Function1<Proveedor?, Unit>' was expected
```

### Qué significa

El callback que recibe tu componente acepta un `Proveedor?` (nullable), pero vos le estás pasando una lambda que espera `Proveedor` (non-null).

### Arreglo recomendado

Cambiá la lambda para aceptar nullable y manejar `null` dentro.

```kotlin
// Antes (falla)
onProveedorSelected = { proveedor: Proveedor ->
    viewModel.onProveedorSelected(proveedor)
}

// Después (ok)
onProveedorSelected = { proveedor: Proveedor? ->
    proveedor?.let { viewModel.onProveedorSelected(it) }
}
```

Si querés permitir "sin proveedor", podés enviar un evento alternativo en el `else`.

## Error 2

```text
actual type is 'Function1<List<Int>, Unit>', but 'Function1<Set<Int>, Unit>' was expected
```

### Qué significa

Tu callback espera `Set<Int>` (sin duplicados), pero le pasás una lambda para `List<Int>`.

### Arreglo recomendado

Ajustá la firma a `Set<Int>` o convertí explícitamente.

```kotlin
// Antes (falla)
onCategorySelectionChanged = { ids: List<Int> ->
    viewModel.onCategorySelectionChanged(ids)
}

// Después (ok)
onCategorySelectionChanged = { ids: Set<Int> ->
    viewModel.onCategorySelectionChanged(ids.toList()) // si el VM aún usa List
}
```

O mejor, alineá todo a `Set<Int>` de punta a punta si representa selección múltiple.

## Ejemplo combinado (líneas aproximadas 211 y 247)

```kotlin
AddStockMainForm(
    // ...
    onProveedorSelected = { proveedor: Proveedor? ->
        proveedor?.let(viewModel::onProveedorSelected)
    },
    onCategorySelectionChanged = { selectedIds: Set<Int> ->
        viewModel.onCategorySelectionChanged(selectedIds)
    }
)
```

Si tu `viewModel.onCategorySelectionChanged` hoy recibe `List<Int>`, cambiá temporalmente a:

```kotlin
viewModel.onCategorySelectionChanged(selectedIds.toList())
```
