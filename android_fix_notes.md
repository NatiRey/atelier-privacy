# Ceramistic: imágenes de bienvenida para modo oscuro en Android

Como agregaste las imágenes `onboarding_welcome_night` y `pizarra_tacita_night`, la forma correcta en Android es usar calificadores de recursos `-night` para que el sistema haga el cambio automático al entrar en modo oscuro.

## 1) Estructura de carpetas recomendada

Ubicá las imágenes así (mismo nombre base para modo claro/oscuro):

```text
app/src/main/res/drawable/onboarding_welcome.png
app/src/main/res/drawable/pizarra_tacita.png

app/src/main/res/drawable-night/onboarding_welcome.png
app/src/main/res/drawable-night/pizarra_tacita.png
```

> Importante: En `drawable-night` **no** uses `_night` en el nombre del archivo. El selector lo hace la carpeta, no el nombre.

## 2) Referencias en las pantallas de inicio

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

