# Diseño en React Native: Box Model, Position y Flexbox

En React Native, todo el diseño de la interfaz se basa en **cajas (`View`)**.  
Estas cajas se organizan y posicionan usando **Box Model**, **Position** y **Flexbox**.

---

## 1. Box Object Model (Box Model)

El **Box Model** define cómo se construye visualmente un componente.

Cada `View` está formada por:

- **Width / Height** → Tamaño del componente
- **Padding** → Espacio interno
- **Border** → Borde que rodea el contenido
- **Margin** → Espacio externo

### Conceptos clave

- `padding` empuja el contenido hacia adentro  
- `margin` separa una caja de otras  
- `borderWidth` y `borderColor` delimitan visualmente el componente  
- `width` y `height` controlan el tamaño exacto  

### Ejemplo

```tsx
<View
  style={{
    width: 150,
    height: 80,
    padding: 16,
    marginTop: 12,
    borderWidth: 3,
    borderColor: "white",
    backgroundColor: "#5856D6",
  }}
>
  <Text>Box</Text>
</View>
```

---

## 2. Width y Height porcentual + Dimensiones de la pantalla

React Native permite usar **porcentajes** y **dimensiones dinámicas**.

### Width y Height en porcentaje

```tsx
<View style={{ width: "60%", height: 80 }} />
```

El porcentaje se calcula respecto al contenedor padre.

### Dimensiones reales de la pantalla

```tsx
import { useWindowDimensions } from "react-native";

const { width, height } = useWindowDimensions();
```

Ejemplo práctico:

```tsx
<View style={{ width: width * 0.6, height: 100 }} />
```

---

## 3. Position en React Native

El sistema de posicionamiento controla cómo se coloca una caja dentro de su contenedor.

### Tipos de posición

#### relative (default)

```tsx
<View style={{ position: "relative", top: 10 }} />
```

#### absolute

```tsx
<View style={{ position: "absolute", top: 10, right: 10 }} />
```

### Usos comunes

- Badges
- Botones flotantes
- Overlays
- Elementos decorativos

---

## 4. Flexbox en React Native

React Native usa Flexbox por defecto.

### flex

```tsx
<View style={{ flex: 1 }} />
<View style={{ flex: 2 }} />
<View style={{ flex: 3 }} />
```

Distribución proporcional del espacio.

---

### flexDirection

```tsx
flexDirection: "column"
flexDirection: "row"
```

---

### alignItems

Alinea los hijos en el eje secundario.

```tsx
alignItems: "flex-start"
alignItems: "center"
alignItems: "flex-end"
```

---

### alignSelf

Permite alinear un solo hijo de forma independiente.

```tsx
<View style={{ alignSelf: "flex-end" }} />
```

---

### flexWrap

Permite que los elementos salten a otra línea.

```tsx
flexWrap: "wrap"
```

---

## Resumen

- Box Model controla tamaño y espacios
- Dimensions permite diseño responsive
- Position controla ubicación precisa
- Flexbox organiza el layout completo
