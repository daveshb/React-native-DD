# Clase de Reforzamiento: TypeScript + React

##  Tabla de Contenido
1. [Tipos Básicos de TypeScript](#1-tipos-básicos-de-typescript)
2. [Interfaces de TypeScript](#2-interfaces-de-typescript)
3. [Funciones: Retorno y Argumentos](#3-funciones-retorno-y-argumentos)
4. [Hook useState](#4-hook-usestate)
5. [Hook useEffect](#5-hook-useeffect)
6. [Custom Hooks](#6-custom-hooks)
7. [Zustand - Estado Global](#7-zustand---estado-global)
8. [Peticiones con Axios](#8-peticiones-con-axios)
9. [Respuestas HTTP](#9-respuestas-http)
10. [Formularios](#10-formularios)

---

## 1. Tipos Básicos de TypeScript

TypeScript nos permite definir tipos de datos para nuestras variables, lo que nos ayuda a prevenir errores en tiempo de desarrollo.

### 1.1 Tipos Primitivos

```typescript
// String - Cadenas de texto
const nombre: string = 'Fernando';
const apellido: string = "Herrera";

// Number - Números (enteros y decimales)
const edad: number = 37;
const precio: number = 19.99;
const temperatura: number = -5;

// Boolean - Verdadero o Falso
const isActive: boolean = true;
const isLoggedIn: boolean = false;
```

### 1.2 Arrays (Arreglos)

```typescript
// Array de strings
const powers: string[] = ['React', 'ReactNative', 'Angular', 'Vue', 'Qwik'];

// Forma alternativa (genéricos)
const numbers: Array<number> = [1, 2, 3, 4, 5];

// Array de booleanos
const flags: boolean[] = [true, false, true];
```

### 1.3 Operador Ternario

El operador ternario es una forma corta de escribir un `if-else`:

```typescript
// Sintaxis: condición ? valorSiVerdadero : valorSiFalso
const edad: number = 20;
const esMayorDeEdad: string = edad >= 18 ? 'Sí' : 'No';

// En React (renderizado condicional)
const isActive: boolean = true;
<div>
  { isActive ? 'Usuario activo' : 'Usuario inactivo' }
</div>
```

### 1.4 Ejemplo Completo en Componente React

```tsx
export const BasicTypes = () => {
  const name: string = 'Fernando';
  const age: number = 37;
  const isActive: boolean = true;
  const powers: string[] = ['React', 'ReactNative', 'Angular', 'Vue'];

  return (
    <>
      <h3>Tipos básicos</h3>
      
      {/* Mostrando variables */}
      { name } { age } { isActive ? 'true' : 'false' }
      <br />
      
      {/* Mostrando array */}
      { powers.join(', ') }
    </>
  )
}
```

### Conceptos Clave:
- **TypeScript** detecta errores en tiempo de desarrollo
- Los **tipos** hacen tu código más predecible y fácil de mantener
- El operador **ternario** es perfecto para renderizado condicional en React

---

## 2. Interfaces de TypeScript

Las interfaces definen la "forma" o estructura que debe tener un objeto. Son como contratos que nuestros objetos deben cumplir.

### 2.1 Definición de Interfaces

```typescript
// Interface simple
interface User {
  id: number;
  name: string;
  email: string;
}

// Interface con propiedades opcionales (?)
interface Person {
  firstName: string;
  lastName: string;
  age: number;
  isAlive?: boolean;  // ← Esta propiedad es opcional
}
```

### 2.2 Interfaces Anidadas

```typescript
interface Address {
  country: string;
  city: string;
  houseNo: number;
}

interface Person {
  firstName: string;
  lastName: string;
  age: number;
  address: Address;  // ← Interface dentro de otra interface
  isAlive?: boolean;
}
```

### 2.3 Uso en Componentes

```tsx
export const ObjectLiterals = () => {
  // TypeScript verifica que el objeto cumpla con la interface
  const person: Person = {
    age: 37,
    firstName: 'Fernando',
    lastName: 'Herrera',
    address: {
      country: 'Canada',
      city: 'Toronto',
      houseNo: 615
    }
    // isAlive es opcional, no es necesario incluirla
  };

  return (
    <>
      <h3>Objetos literales</h3>
      <pre>
        { JSON.stringify(person, null, 2) }
      </pre>
    </>
  );
};
```

### 2.4 Interfaces en Props de Componentes

```tsx
// Definimos la interface de las props
interface Props {
  user: User;
  isActive?: boolean;
}

// Usamos la interface en el componente
export const UserCard = ({ user, isActive = true }: Props) => {
  return (
    <div>
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      {isActive && <span>Activo</span>}
    </div>
  );
};
```

### 2.5 Ejemplo de Interface para API Response

```typescript
// Interface para respuesta de API
interface User {
  id: number;
  email: string;
  first_name: string;
  last_name: string;
  avatar: string;
}

interface ReqResUserListResponse {
  page: number;
  per_page: number;
  total: number;
  total_pages: number;
  data: User[];
}
```

###  Conceptos Clave:
- Las **interfaces** definen la estructura de objetos
- Propiedades **opcionales** se marcan con `?`
- Son fundamentales para **type safety** en TypeScript
- Se usan en **props de componentes** y **respuestas de APIs**

---

## 3. Funciones: Retorno y Argumentos

En TypeScript podemos tipar tanto los argumentos que recibe una función como el valor que retorna.

### 3.1 Funciones con Tipos en Argumentos

```typescript
// Función que recibe dos números
function sumar(a: number, b: number) {
  return a + b;
}

// Arrow function
const restar = (a: number, b: number) => {
  return a - b;
}

// Uso
const resultado = sumar(5, 3);  // 8
```

### 3.2 Funciones con Tipo de Retorno

```typescript
// Especificamos que retorna un string
const addTwoNumber = (a: number, b: number): string => {
  return `${a + b}`;
}

// Especificamos que retorna un number
const multiplicar = (a: number, b: number): number => {
  return a * b;
}

// Función que no retorna nada (void)
const saludar = (nombre: string): void => {
  console.log(`Hola ${nombre}`);
}
```

### 3.3 Argumentos Opcionales

```typescript
// El argumento 'apellido' es opcional
const crearNombreCompleto = (nombre: string, apellido?: string): string => {
  if (apellido) {
    return `${nombre} ${apellido}`;
  }
  return nombre;
}

// Uso
crearNombreCompleto('Fernando');              // "Fernando"
crearNombreCompleto('Fernando', 'Herrera');   // "Fernando Herrera"
```

### 3.4 Argumentos con Valores por Defecto

```typescript
const calcularPrecio = (precio: number, impuesto: number = 0.16): number => {
  return precio + (precio * impuesto);
}

// Uso
calcularPrecio(100);      // 116 (usa el impuesto por defecto)
calcularPrecio(100, 0.10); // 110 (usa el impuesto proporcionado)
```

### 3.5 Funciones con Objetos como Argumentos

```typescript
interface Options {
  name: string;
  age: number;
  isActive?: boolean;
}

const crearUsuario = (options: Options): string => {
  const { name, age, isActive = true } = options;
  return `Usuario: ${name}, Edad: ${age}, Activo: ${isActive}`;
}

// Uso
crearUsuario({ name: 'Fernando', age: 37 });
```

### 3.6 Ejemplo Completo en Componente

```tsx
export const BasicFunctions = () => {
  // Función que suma y retorna string
  const addTwoNumber = (a: number, b: number): string => {
    return `${a + b}`;
  }

  // Función que valida edad
  const isAdult = (age: number): boolean => {
    return age >= 18;
  }

  return (
    <>
      <h3>Funciones</h3>
      <span>El resultado de sumar: {addTwoNumber(2, 8)}</span>
      <br />
      <span>¿Es mayor de edad? {isAdult(20) ? 'Sí' : 'No'}</span>
    </>
  )
}
```

###  Conceptos Clave:
- Siempre **tipa los argumentos** de tus funciones
- Especifica el **tipo de retorno** para mayor claridad
- Usa **argumentos opcionales** con `?`
- Los **valores por defecto** hacen tu código más flexible

---

## 4. Hook useState

`useState` es el hook más básico de React. Nos permite agregar estado a nuestros componentes funcionales.

### 4.1 Sintaxis Básica

```tsx
import { useState } from 'react';

// Sintaxis: const [variable, función] = useState(valorInicial);
const [count, setCount] = useState(0);
```

### 4.2 useState sin TypeScript

```tsx
import { useState } from 'react';

export const Counter = () => {
  // React infiere el tipo automáticamente
  const [count, setCount] = useState(10);

  return (
    <>
      <h3>Contador: <small>{count}</small></h3>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </>
  )
}
```

### 4.3 useState con TypeScript (Tipado Explícito)

```tsx
import { useState } from 'react';

export const Counter = () => {
  // Especificamos explícitamente que es un number
  const [count, setCount] = useState<number>(10);

  const increaseBy = (value: number) => {
    setCount(count + value);
  }

  return (
    <>
      <h3>Contador: <small>{count}</small></h3>
      
      <div>
        <button onClick={() => increaseBy(+1)}>+1</button>
        <button onClick={() => increaseBy(-1)}>-1</button>
      </div>
    </>
  )
}
```

### 4.4 useState con Objetos

```tsx
interface User {
  name: string;
  age: number;
  email: string;
}

export const UserComponent = () => {
  const [user, setUser] = useState<User>({
    name: 'Fernando',
    age: 37,
    email: 'fernando@google.com'
  });

  const updateName = (newName: string) => {
    setUser({
      ...user,           // Mantenemos las demás propiedades
      name: newName      // Solo actualizamos el nombre
    });
  }

  return (
    <div>
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <button onClick={() => updateName('John')}>Cambiar nombre</button>
    </div>
  );
}
```

### 4.5 useState con Arrays

```tsx
export const TodoList = () => {
  const [todos, setTodos] = useState<string[]>(['Aprender React', 'Aprender TypeScript']);

  const addTodo = (newTodo: string) => {
    setTodos([...todos, newTodo]);  // Agregamos un nuevo todo
  }

  const removeTodo = (index: number) => {
    setTodos(todos.filter((_, i) => i !== index));  // Eliminamos por índice
  }

  return (
    <>
      <h3>Lista de Tareas</h3>
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>
            {todo}
            <button onClick={() => removeTodo(index)}>X</button>
          </li>
        ))}
      </ul>
      <button onClick={() => addTodo('Nueva tarea')}>Agregar</button>
    </>
  );
}
```

### 4.6 useState con Estado Opcional

```tsx
export const UserProfile = () => {
  // Puede ser User o undefined
  const [user, setUser] = useState<User | undefined>();

  const loadUser = () => {
    setUser({
      name: 'Fernando',
      age: 37,
      email: 'fernando@google.com'
    });
  }

  return (
    <>
      {/* Renderizado condicional */}
      {user ? (
        <div>
          <h3>{user.name}</h3>
          <p>{user.email}</p>
        </div>
      ) : (
        <button onClick={loadUser}>Cargar Usuario</button>
      )}
    </>
  );
}
```

### 4.7 Renderizado Condicional con useState

```tsx
export const LoginForm = () => {
  const [isLoggedIn, setIsLoggedIn] = useState<boolean>(false);
  const [username, setUsername] = useState<string>('');

  const handleLogin = () => {
    setIsLoggedIn(true);
  }

  const handleLogout = () => {
    setIsLoggedIn(false);
    setUsername('');
  }

  return (
    <>
      <h3>Login Page</h3>
      
      {/* Renderizado condicional con operador ternario */}
      {isLoggedIn ? (
        <div>
          <p>Bienvenido, {username}!</p>
          <button onClick={handleLogout}>Cerrar Sesión</button>
        </div>
      ) : (
        <div>
          <input 
            type="text" 
            value={username}
            onChange={(e) => setUsername(e.target.value)}
            placeholder="Usuario"
          />
          <button onClick={handleLogin}>Iniciar Sesión</button>
        </div>
      )}
      
      {/* Renderizado condicional con && */}
      {isLoggedIn && <p>Estás autenticado</p>}
    </>
  );
}
```

###  Conceptos Clave de useState:

#### ¿Qué es el estado?
- El **estado** es información que puede cambiar en tu componente
- Cuando el estado cambia, React **re-renderiza** el componente
- Solo se debe modificar con la función `setState`, nunca directamente

#### Reglas importantes:
1. **No modificar el estado directamente:**
   ```tsx
   // ❌ INCORRECTO
   count = count + 1;
   
   // ✅ CORRECTO
   setCount(count + 1);
   ```

2. **Con objetos y arrays, crear copias nuevas:**
   ```tsx
   // ❌ INCORRECTO
   user.name = 'Nuevo nombre';
   setUser(user);
   
   // ✅ CORRECTO
   setUser({ ...user, name: 'Nuevo nombre' });
   ```

3. **El estado es asíncrono:**
   ```tsx
   setCount(5);
   console.log(count); // ⚠️ Puede no ser 5 inmediatamente
   ```

#### Renderizado Condicional:
- **Operador ternario:** `condición ? elementoSiTrue : elementoSiFalse`
- **Operador &&:** `condición && elemento` (solo muestra si es true)
- **If-else tradicional:** Para lógica más compleja fuera del JSX

---

## 5. Hook useEffect

`useEffect` es un hook que nos permite ejecutar código cuando el componente se monta, actualiza o desmonta. Es útil para efectos secundarios como peticiones HTTP, suscripciones, timers, etc.

### 5.1 Sintaxis Básica

```tsx
import { useEffect } from 'react';

useEffect(() => {
  // Código que se ejecuta
}, [dependencias]);
```

### 5.2 useEffect sin Dependencias (Se ejecuta en cada render)

```tsx
import { useState, useEffect } from 'react';

export const Component = () => {
  const [count, setCount] = useState(0);

  // ⚠️ Se ejecuta en CADA render (no recomendado)
  useEffect(() => {
    console.log('El componente se renderizó');
  });

  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}
```

### 5.3 useEffect con Array Vacío (Se ejecuta solo al montar)

```tsx
import { useState, useEffect } from 'react';

export const Component = () => {
  const [data, setData] = useState(null);

  // ✅ Se ejecuta UNA SOLA VEZ cuando el componente se monta
  useEffect(() => {
    console.log('Componente montado');
    
    // Aquí puedes hacer peticiones HTTP
    fetch('https://api.ejemplo.com/data')
      .then(res => res.json())
      .then(data => setData(data));
  }, []); // ← Array vacío = solo al montar

  return <div>Data: {data}</div>;
}
```

### 5.4 useEffect con Dependencias Específicas

```tsx
import { useState, useEffect } from 'react';

export const SearchComponent = () => {
  const [searchTerm, setSearchTerm] = useState('');
  const [results, setResults] = useState([]);

  // ✅ Se ejecuta cuando 'searchTerm' cambia
  useEffect(() => {
    console.log(`Buscando: ${searchTerm}`);
    
    if (searchTerm) {
      // Realizar búsqueda
      fetch(`https://api.ejemplo.com/search?q=${searchTerm}`)
        .then(res => res.json())
        .then(data => setResults(data));
    }
  }, [searchTerm]); // ← Se ejecuta cuando searchTerm cambia

  return (
    <div>
      <input 
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Buscar..."
      />
      <ul>
        {results.map(item => <li key={item.id}>{item.name}</li>)}
      </ul>
    </div>
  );
}
```

### 5.5 Función de Limpieza (Cleanup)

```tsx
import { useState, useEffect } from 'react';

export const TimerComponent = () => {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    console.log('Iniciando timer');
    
    // Crear el intervalo
    const interval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);

    // ✅ Función de limpieza (se ejecuta al desmontar o antes de re-ejecutar)
    return () => {
      console.log('Limpiando timer');
      clearInterval(interval);
    };
  }, []); // Solo al montar

  return <div>Segundos: {seconds}</div>;
}
```

### 5.6 Ejemplo Real: Login con Timeout

```tsx
import { useEffect, useState } from 'react';

export const LoginPage = () => {
  const [authStatus, setAuthStatus] = useState('checking');
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Simular verificación de autenticación
    const timeout = setTimeout(() => {
      setAuthStatus('unauthenticated');
    }, 1500);

    // Limpieza del timeout
    return () => clearTimeout(timeout);
  }, []); // Solo al montar

  const handleLogin = () => {
    setAuthStatus('authenticated');
    setUser({ name: 'John Doe', email: 'john@example.com' });
  };

  if (authStatus === 'checking') {
    return <h3>Loading...</h3>;
  }

  return (
    <>
      <h3>Login Page</h3>
      {authStatus === 'authenticated' ? (
        <div>
          <p>Autenticado como: {user?.name}</p>
          <button onClick={() => setAuthStatus('unauthenticated')}>
            Logout
          </button>
        </div>
      ) : (
        <button onClick={handleLogin}>Login</button>
      )}
    </>
  );
};
```

### 5.7 Múltiples useEffect en un Componente

```tsx
import { useState, useEffect } from 'react';

export const MultiEffectComponent = () => {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');

  // Efecto 1: Se ejecuta cuando 'count' cambia
  useEffect(() => {
    console.log(`Count cambió a: ${count}`);
    document.title = `Clicks: ${count}`;
  }, [count]);

  // Efecto 2: Se ejecuta cuando 'name' cambia
  useEffect(() => {
    console.log(`Name cambió a: ${name}`);
  }, [name]);

  // Efecto 3: Solo al montar
  useEffect(() => {
    console.log('Componente montado');
    return () => console.log('Componente desmontado');
  }, []);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <input value={name} onChange={(e) => setName(e.target.value)} />
    </>
  );
}
```

###  Conceptos Clave de useEffect:

#### Casos de uso principales:
1. **Peticiones HTTP/API** al montar el componente
2. **Suscripciones** a eventos (WebSockets, listeners)
3. **Timers** y intervals
4. **Sincronización** con sistemas externos
5. **Actualizar** el documento (title, localStorage, etc.)

#### Tipos de dependencias:
- **Sin array:** Se ejecuta en cada render (evitar)
- **Array vacío `[]`:** Se ejecuta solo al montar
- **Con dependencias `[var1, var2]`:** Se ejecuta cuando cambian

#### Función de limpieza:
- Se ejecuta **antes de desmontar** el componente
- Se ejecuta **antes de volver a ejecutar** el efecto
- Usar para limpiar: timers, suscripciones, listeners

#### Reglas importantes:
```tsx
// ❌ NO hacer esto (infinite loop)
useEffect(() => {
  setCount(count + 1);
}); // Sin dependencias = loop infinito

// ❌ NO olvidar dependencias
useEffect(() => {
  console.log(searchTerm); // Usa searchTerm
}, []); // ⚠️ Falta searchTerm en dependencias

// ✅ CORRECTO
useEffect(() => {
  console.log(searchTerm);
}, [searchTerm]); // ✅ Incluye todas las dependencias
```

---

## 6. Custom Hooks

Los Custom Hooks son funciones que nos permiten **reutilizar lógica** entre componentes. Deben comenzar con la palabra "use" y pueden usar otros hooks.

### 6.1 ¿Por qué usar Custom Hooks?

**Problema:** Código duplicado en múltiples componentes
```tsx
// Componente 1
const Component1 = () => {
  const [count, setCount] = useState(0);
  const increaseBy = (value) => {
    if (count + value < 0) return;
    setCount(count + value);
  };
  // ...
}

// Componente 2
const Component2 = () => {
  const [count, setCount] = useState(0);
  const increaseBy = (value) => {
    if (count + value < 0) return;
    setCount(count + value);
  };
  // ... mismo código repetido
}
```

**Solución:** Custom Hook
```tsx
// hooks/useCounter.tsx
import { useState } from 'react';

export const useCounter = (initialValue = 0) => {
  const [count, setCount] = useState(initialValue);

  const increaseBy = (value) => {
    const newValue = count + value;
    if (newValue < 0) return;
    setCount(count + value);
  };

  return {
    count,
    increaseBy,
  };
};
```

### 6.2 Estructura Básica de un Custom Hook

```tsx
import { useState } from 'react';

// 1. El nombre DEBE empezar con "use"
// 2. Puede recibir argumentos
// 3. Puede usar otros hooks
// 4. DEBE retornar algo (objeto, array, valor, etc.)

export const useCustomHook = (initialValue) => {
  const [state, setState] = useState(initialValue);

  const someMethod = () => {
    // Lógica
  };

  // Retornar lo que necesiten los componentes
  return {
    // Properties
    state,
    // Methods
    someMethod,
  };
};
```

### 6.3 Custom Hook: useCounter

```tsx
// hooks/useCounter.tsx
import { useState } from 'react';

interface Options {
  initialValue?: number;
}

export const useCounter = ({ initialValue = 0 }: Options) => {
  const [count, setCount] = useState<number>(initialValue);

  const increaseBy = (value: number) => {
    const newValue = count + value;
    if (newValue < 0) return; // No permitir valores negativos
    
    setCount(count + value);
  }

  const reset = () => {
    setCount(initialValue);
  }

  return {
    // Properties
    count,
    
    // Methods
    increaseBy,
    reset,
  }
}
```

**Uso del Custom Hook:**
```tsx
import { useCounter } from '../hooks/useCounter';

export const CounterWithHook = () => {
  // Usar el custom hook
  const { count, increaseBy, reset } = useCounter({
    initialValue: 5
  });

  return (
    <>
      <h3>Contador: <small>{count}</small></h3>
      
      <div>
        <button onClick={() => increaseBy(+1)}>+1</button>
        <button onClick={() => increaseBy(-1)}>-1</button>
        <button onClick={reset}>Reset</button>
      </div>
    </>
  )
}
```

### 6.4 Custom Hook: useForm

```tsx
// hooks/useForm.tsx
import { useState, ChangeEvent } from 'react';

export const useForm = <T extends Record<string, any>>(initialForm: T) => {
  const [formData, setFormData] = useState(initialForm);

  const onChange = (event: ChangeEvent<HTMLInputElement>) => {
    const { name, value } = event.target;
    
    setFormData({
      ...formData,
      [name]: value,
    });
  };

  const resetForm = () => {
    setFormData(initialForm);
  };

  return {
    // Properties
    formData,
    
    // Methods
    onChange,
    resetForm,
  };
};
```

**Uso:**
```tsx
import { useForm } from '../hooks/useForm';

interface LoginForm {
  email: string;
  password: string;
}

export const LoginForm = () => {
  const { formData, onChange, resetForm } = useForm<LoginForm>({
    email: '',
    password: ''
  });

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log(formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={onChange}
        placeholder="Email"
      />
      <input
        type="password"
        name="password"
        value={formData.password}
        onChange={onChange}
        placeholder="Password"
      />
      <button type="submit">Login</button>
      <button type="button" onClick={resetForm}>Reset</button>
    </form>
  );
};
```

### 6.5 Custom Hook: useFetch

```tsx
// hooks/useFetch.tsx
import { useState, useEffect } from 'react';

interface FetchState<T> {
  data: T | null;
  loading: boolean;
  error: string | null;
}

export const useFetch = <T,>(url: string) => {
  const [state, setState] = useState<FetchState<T>>({
    data: null,
    loading: true,
    error: null,
  });

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(url);
        const data = await response.json();
        
        setState({
          data,
          loading: false,
          error: null,
        });
      } catch (error) {
        setState({
          data: null,
          loading: false,
          error: 'Error al cargar los datos',
        });
      }
    };

    fetchData();
  }, [url]);

  return state;
};
```

**Uso:**
```tsx
import { useFetch } from '../hooks/useFetch';

interface User {
  id: number;
  name: string;
  email: string;
}

export const UsersList = () => {
  const { data, loading, error } = useFetch<User[]>('https://api.example.com/users');

  if (loading) return <p>Cargando...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {data?.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};
```

###  Conceptos Clave de Custom Hooks:

#### Ventajas:
- ✅ **Reutilización** de lógica entre componentes
- ✅ **Código más limpio** y organizado
- ✅ **Fácil de testear** de forma aislada
- ✅ **Separación de responsabilidades**

#### Reglas:
1. El nombre **DEBE** empezar con "use"
2. Solo se pueden llamar en el nivel superior (no dentro de loops, condicionales, etc.)
3. Solo se pueden llamar desde componentes de React u otros custom hooks

#### Buenas prácticas:
- Crear una carpeta `hooks/` para organizarlos
- Documentar qué reciben y qué retornan
- Usar TypeScript para tipar correctamente
- Mantenerlos simples y con una sola responsabilidad

---

## 7. Zustand - Estado Global

Zustand es una librería de manejo de estado global **simple, rápida y escalable**. Es más sencilla que Redux y perfecta para React.

### 7.1 ¿Qué es Estado Global?

**Estado Local (useState):**
- Solo accesible en un componente
- Se pasa a hijos mediante props
- Puede ser tedioso con muchos niveles

**Estado Global (Zustand):**
- Accesible desde cualquier componente
- No necesita pasar props
- Perfecto para: autenticación, tema, carrito de compras, etc.

### 7.2 Instalación

```bash
npm install zustand
# o
yarn add zustand
```

### 7.3 Crear un Store Básico

```tsx
// store/counter.store.ts
import { create } from 'zustand';

// 1. Definir la interface del estado
interface CounterState {
  // Properties (estado)
  count: number;
  
  // Methods (acciones)
  increment: () => void;
  decrement: () => void;
  reset: () => void;
}

// 2. Crear el store
export const useCounterStore = create<CounterState>()((set) => ({
  // Estado inicial
  count: 0,
  
  // Acciones
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));
```

**Usar el Store:**
```tsx
import { useCounterStore } from '../store/counter.store';

export const CounterComponent = () => {
  // Obtener estado y acciones
  const count = useCounterStore((state) => state.count);
  const increment = useCounterStore((state) => state.increment);
  const decrement = useCounterStore((state) => state.decrement);
  const reset = useCounterStore((state) => state.reset);

  return (
    <div>
      <h3>Counter: {count}</h3>
      <button onClick={increment}>+1</button>
      <button onClick={decrement}>-1</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
};
```

### 7.4 Store de Autenticación (Login/Logout)

```tsx
// store/auth.store.ts
import { create } from 'zustand';

interface AuthState {
  // Estado
  status: 'authenticated' | 'unauthenticated' | 'checking';
  token?: string;
  user?: {
    name: string;
    email: string;
  };

  // Acciones
  login: (email: string, password: string) => void;
  logout: () => void;
}

export const useAuthStore = create<AuthState>()((set) => ({
  // Estado inicial
  status: 'checking',
  token: undefined,
  user: undefined,

  // Login
  login: (email: string, password: string) => {
    // Aquí normalmente harías una petición a tu API
    // Por ahora, simulamos el login
    set({
      status: 'authenticated',
      token: 'ABC123',
      user: {
        name: 'John Doe',
        email: email,
      }
    });
  },

  // Logout
  logout: () => {
    set({
      status: 'unauthenticated',
      token: undefined,
      user: undefined
    });
  },
}));
```

**Usar el Store de Autenticación:**
```tsx
import { useEffect } from 'react';
import { useAuthStore } from '../store/auth.store';

export const LoginPage = () => {
  // Obtener estado y acciones del store
  const authStatus = useAuthStore((state) => state.status);
  const user = useAuthStore((state) => state.user);
  const login = useAuthStore((state) => state.login);
  const logout = useAuthStore((state) => state.logout);

  // Simular verificación inicial
  useEffect(() => {
    setTimeout(() => {
      logout();
    }, 1500);
  }, []);

  if (authStatus === 'checking') {
    return <h3>Loading...</h3>;
  }

  return (
    <>
      <h3>Login Page</h3>

      {/* Renderizado condicional */}
      {authStatus === 'authenticated' ? (
        <div>
          <p>Autenticado como:</p>
          <pre>{JSON.stringify(user, null, 2)}</pre>
          <button onClick={logout}>Logout</button>
        </div>
      ) : (
        <div>
          <p>No Autenticado</p>
          <button onClick={() => login('fernando@google.com', '123')}>
            Login
          </button>
        </div>
      )}
    </>
  );
};
```

### 7.5 Acceder al Store desde Múltiples Componentes

```tsx
// Componente 1: Navbar
import { useAuthStore } from '../store/auth.store';

export const Navbar = () => {
  const user = useAuthStore((state) => state.user);
  const logout = useAuthStore((state) => state.logout);

  return (
    <nav>
      {user && (
        <>
          <span>Hola, {user.name}</span>
          <button onClick={logout}>Salir</button>
        </>
      )}
    </nav>
  );
};

// Componente 2: Profile
import { useAuthStore } from '../store/auth.store';

export const Profile = () => {
  const user = useAuthStore((state) => state.user);

  return (
    <div>
      <h2>Mi Perfil</h2>
      <p>Email: {user?.email}</p>
    </div>
  );
};

// Ambos componentes acceden al MISMO estado global
```

### 7.6 Store con Acciones Asíncronas

```tsx
// store/products.store.ts
import { create } from 'zustand';
import axios from 'axios';

interface Product {
  id: number;
  name: string;
  price: number;
}

interface ProductsState {
  // Estado
  products: Product[];
  loading: boolean;
  error: string | null;

  // Acciones
  fetchProducts: () => Promise<void>;
  addProduct: (product: Product) => void;
}

export const useProductsStore = create<ProductsState>()((set) => ({
  // Estado inicial
  products: [],
  loading: false,
  error: null,

  // Acción asíncrona
  fetchProducts: async () => {
    set({ loading: true, error: null });
    
    try {
      const { data } = await axios.get('https://api.example.com/products');
      set({ products: data, loading: false });
    } catch (error) {
      set({ 
        error: 'Error al cargar productos', 
        loading: false 
      });
    }
  },

  // Acción síncrona
  addProduct: (product) => {
    set((state) => ({
      products: [...state.products, product]
    }));
  },
}));
```

**Uso:**
```tsx
import { useEffect } from 'react';
import { useProductsStore } from '../store/products.store';

export const ProductsList = () => {
  const { products, loading, error, fetchProducts } = useProductsStore();

  useEffect(() => {
    fetchProducts();
  }, []);

  if (loading) return <p>Cargando productos...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {products.map(product => (
        <li key={product.id}>
          {product.name} - ${product.price}
        </li>
      ))}
    </ul>
  );
};
```

### 7.7 Seleccionar Solo lo Necesario (Optimización)

```tsx
// ❌ MAL: Se re-renderiza con cualquier cambio en el store
export const Component = () => {
  const store = useAuthStore(); // Todo el store
  return <div>{store.user?.name}</div>;
};

// ✅ BIEN: Solo se re-renderiza cuando 'user' cambia
export const Component = () => {
  const user = useAuthStore((state) => state.user); // Solo 'user'
  return <div>{user?.name}</div>;
};

// ✅ MEJOR: Múltiples selecciones
export const Component = () => {
  const user = useAuthStore((state) => state.user);
  const logout = useAuthStore((state) => state.logout);
  
  return (
    <div>
      <p>{user?.name}</p>
      <button onClick={logout}>Salir</button>
    </div>
  );
};
```

### 7.8 Persistencia con localStorage

```tsx
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface SettingsState {
  theme: 'light' | 'dark';
  language: string;
  toggleTheme: () => void;
}

export const useSettingsStore = create<SettingsState>()(
  persist(
    (set) => ({
      theme: 'light',
      language: 'es',
      
      toggleTheme: () => {
        set((state) => ({
          theme: state.theme === 'light' ? 'dark' : 'light'
        }));
      },
    }),
    {
      name: 'settings-storage', // Nombre en localStorage
    }
  )
);
```

###  Conceptos Clave de Zustand:

#### Ventajas:
- ✅ **Simple**: Menos código que Redux
- ✅ **Rápido**: Muy buen rendimiento
- ✅ **Sin Provider**: No necesitas wrappers
- ✅ **TypeScript**: Excelente soporte
- ✅ **DevTools**: Compatible con Redux DevTools

#### Estructura de un Store:
```tsx
create<Interface>()((set, get) => ({
  // 1. Estado inicial
  property: value,
  
  // 2. Acciones síncronas
  action: () => set({ property: newValue }),
  
  // 3. Acciones asíncronas
  asyncAction: async () => {
    const data = await fetch(...);
    set({ property: data });
  },
  
  // 4. Acciones con estado previo
  increment: () => set((state) => ({ 
    count: state.count + 1 
  })),
}));
```

#### Cuándo usar Zustand:
- ✅ Autenticación (usuario, token)
- ✅ Carrito de compras
- ✅ Tema/configuración de la app
- ✅ Datos que se comparten entre muchos componentes
- ❌ Estado muy local (mejor useState)

---

## 8. Peticiones con Axios

Axios es una librería para realizar peticiones HTTP de forma sencilla. Soporta promesas, interceptores y funciona muy bien con TypeScript.

### 8.1 Instalación

```bash
npm install axios
# o
yarn add axios
```

### 8.2 Tipos e Interfaces de Respuesta

Usar interfaces tipadas hace que `axios` retorne datos con type safety.

```ts
// Ejemplo de respuesta tipada (ReqRes)
export interface ReqResUserListResponse {
  page:        number;
  per_page:    number;
  total:       number;
  total_pages: number;
  data:        User[];
}

export interface User {
  id:         number;
  email:      string;
  first_name: string;
  last_name:  string;
  avatar:     string;
}
```

### 8.3 GET tipado con Axios

```tsx
import axios from 'axios';

// GET con genérico de TypeScript para tipar la respuesta
export const loadUsers = async (page: number = 1): Promise<User[]> => {
  try {
    const { data } = await axios.get<ReqResUserListResponse>('https://reqres.in/api/users', {
      params: { page }
    });
    return data.data; // ← Users ya tipados
  } catch (error) {
    console.log(error);
    return [];
  }
};
```

### 8.4 POST con Axios (body tipado)

```tsx
import axios from 'axios';

interface LoginBody {
  email: string;
  password: string;
}

interface LoginResponse {
  token: string;
  user: { name: string; email: string };
}

export const loginRequest = async (body: LoginBody): Promise<LoginResponse> => {
  const { data } = await axios.post<LoginResponse>('https://api.ejemplo.com/login', body);
  return data;
};
```

### 8.5 Manejo de errores y estados (loading)

```tsx
import { useEffect, useState } from 'react';
import axios from 'axios';

export const UsersPage = () => {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState<boolean>(false);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchUsers = async () => {
      setLoading(true);
      setError(null);
      try {
        const { data } = await axios.get<ReqResUserListResponse>('https://reqres.in/api/users');
        setUsers(data.data);
      } catch (e) {
        setError('No se pudieron cargar los usuarios');
      } finally {
        setLoading(false);
      }
    };

    fetchUsers();
  }, []);

  if (loading) return <p>Cargando...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <>
      <h3>Usuarios:</h3>
      <table>
        <thead>
          <tr>
            <th>Avatar</th>
            <th>Nombre</th>
            <th>Email</th>
          </tr>
        </thead>
        <tbody>
          {users.map((user) => (
            <tr key={user.id}>
              <td><img style={{ width: '50px' }} src={user.avatar} alt="User avatar" /></td>
              <td>{user.first_name} {user.last_name}</td>
              <td>{user.email}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </>
  );
};
```

### 8.6 Paginación con Axios y Custom Hook

```tsx
import axios from 'axios';
import { useState, useRef, useEffect } from 'react';

const loadUsers = async (page: number = 1): Promise<User[]> => {
  try {
    const { data } = await axios.get<ReqResUserListResponse>('https://reqres.in/api/users', {
      params: { page }
    });
    return data.data;
  } catch (error) {
    console.log(error);
    return [];
  }
};

export const useUsers = () => {
  const [users, setUsers] = useState<User[]>([]);
  const currentPageRef = useRef(1);

  useEffect(() => {
    loadUsers(currentPageRef.current).then(setUsers);
  }, []);

  const nextPage = async () => {
    currentPageRef.current++;
    const newUsers = await loadUsers(currentPageRef.current);
    if (newUsers.length > 0) {
      setUsers(newUsers);
    } else {
      currentPageRef.current--; // revertir si no hay datos
    }
  };

  const prevPage = async () => {
    if (currentPageRef.current <= 1) return;
    currentPageRef.current--;
    const newUsers = await loadUsers(currentPageRef.current);
    setUsers(newUsers);
  };

  return { users, nextPage, prevPage };
};
```

**Uso del hook con paginación:**
```tsx
export const UsersWithPagination = () => {
  const { users, nextPage, prevPage } = useUsers();

  return (
    <>
      <h3>Usuarios:</h3>
      <table>
        <thead>
          <tr>
            <th>Avatar</th>
            <th>Nombre</th>
            <th>Email</th>
          </tr>
        </thead>
        <tbody>
          {users.map((user) => (
            <tr key={user.id}>
              <td><img style={{ width: '50px' }} src={user.avatar} alt="User avatar" /></td>
              <td>{user.first_name} {user.last_name}</td>
              <td>{user.email}</td>
            </tr>
          ))}
        </tbody>
      </table>

      <div>
        <button onClick={prevPage}>Prev</button>
        <button onClick={nextPage}>Next</button>
      </div>
    </>
  );
};
```

### 8.7 Interceptores (opcional) y baseURL

```ts
import axios from 'axios';

export const api = axios.create({
  baseURL: 'https://reqres.in/api',
  timeout: 10000,
});

// Interceptor de request
api.interceptors.request.use((config) => {
  // Agregar token, logs, etc.
  return config;
});

// Interceptor de response
api.interceptors.response.use(
  (response) => response,
  (error) => {
    // Manejo centralizado de errores
    return Promise.reject(error);
  }
);
```

###  Conceptos Clave de Axios
- **`data`**: cuerpo de la respuesta (ya parseado)
- **`status`**: código HTTP (200, 404, 500)
- **`params`**: enviar query params `?page=1`
- **`headers`**: autenticación, content-type, etc.
- **Genéricos TS**: `axios.get<TipoRespuesta>()` para obtener type safety

## 9. Respuestas HTTP

Entender las respuestas HTTP te ayuda a manejar correctamente estados de éxito, error, timeouts y renderizar la UI apropiadamente.

### 9.1 Conceptos clave
- **Códigos 2xx**: Éxito (200 OK, 201 Created)
- **Códigos 3xx**: Redirecciones
- **Códigos 4xx**: Errores del cliente (400 Bad Request, 401 Unauthorized, 404 Not Found)
- **Códigos 5xx**: Errores del servidor (500 Internal Server Error)

### 9.2 AxiosResponse y AxiosError (tipado)

```ts
import type { AxiosResponse, AxiosError } from 'axios';

type UsersResponse = AxiosResponse<ReqResUserListResponse>;
type UsersError = AxiosError<{ error?: string; message?: string }>; // Ejemplo
```

Propiedades útiles en `AxiosResponse<T>`:
- `data`: cuerpo parseado (tipo T)
- `status`: código HTTP
- `statusText`: texto del estado
- `headers`: encabezados de la respuesta

### 9.3 Manejo de estados por `status`

```tsx
import axios from 'axios';
import { useEffect, useState } from 'react';

export const UsersByStatus = () => {
  const [users, setUsers] = useState<User[]>([]);
  const [status, setStatus] = useState<number | null>(null);
  const [errorMsg, setErrorMsg] = useState<string | null>(null);

  const fetchUsers = async () => {
    try {
      const resp = await axios.get<ReqResUserListResponse>('https://reqres.in/api/users');
      setStatus(resp.status);
      setUsers(resp.data.data);
    } catch (err) {
      if (axios.isAxiosError(err)) {
        setStatus(err.response?.status ?? null);
        setErrorMsg(err.response?.data?.message ?? 'Error desconocido');
      } else {
        setErrorMsg('Error no Axios');
      }
    }
  };

  useEffect(() => { fetchUsers(); }, []);

  // Renderizado condicional con ternario
  return (
    <div>
      <h3>Usuarios (status: {status ?? 'N/A'})</h3>

      {status && status >= 200 && status < 300 ? (
        <ul>
          {users.map(u => (
            <li key={u.id}>{u.first_name} {u.last_name}</li>
          ))}
        </ul>
      ) : (
        <p style={{ color: 'tomato' }}>{errorMsg ?? 'Cargando / error'}</p>
      )}
    </div>
  );
};
```

### 9.4 Diferenciar tipos de error

```ts
try {
  await axios.get('/endpoint');
} catch (err) {
  if (axios.isAxiosError(err)) {
    if (err.response) {
      // El servidor respondió con status != 2xx
      console.log('Server error', err.response.status, err.response.data);
    } else if (err.request) {
      // No hubo respuesta (timeout, red, CORS)
      console.log('Network error / no response');
    } else {
      // Error al configurar la petición
      console.log('Config error', err.message);
    }
  } else {
    console.log('Error desconocido');
  }
}
```

### 9.5 `validateStatus` y timeouts

```ts
axios.get('https://reqres.in/api/users', {
  timeout: 10000,
  validateStatus: (status) => status >= 200 && status < 400, // Tratar 3xx como éxito
});
```

### 9.6 UI basada en códigos

```tsx
const StatusMessage = ({ status }: { status?: number }) => {
  if (!status) return <span>Sin estado</span>;

  const msg = status >= 200 && status < 300
    ? 'Operación exitosa'
    : status >= 400 && status < 500
      ? 'Error del cliente'
      : status >= 500
        ? 'Error del servidor'
        : 'Otro estado';

  return <span>{msg} (HTTP {status})</span>;
};
```

### 9.7 Buenas prácticas
- Tipar respuestas y errores con interfaces
- Mostrar estados de `loading`, `success`, `error`
- Evitar `catch` silenciosos: loguear y notificar
- Usar interceptores para manejo centralizado
- Definir `baseURL` y `timeout` en un cliente común



## 10. Formularios

Formularios en React se pueden manejar de dos formas principales: **controlados** (con `useState`) y usando librerías como **React Hook Form** que optimizan rendimiento y simplifican validaciones.

### 10.1 Formulario Controlado (useState)

```tsx
import { useState } from 'react';

export const SimpleLoginControlled = () => {
  const [email, setEmail] = useState<string>('');
  const [password, setPassword] = useState<string>('');
  const [submitted, setSubmitted] = useState<boolean>(false);

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    setSubmitted(true);
    // Aquí podrías llamar a tu API
    console.log({ email, password }); // ← Valores del formulario
  };

  return (
    <form onSubmit={handleSubmit}>
      <h3>Login (controlado)</h3>
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button type="submit">Ingresar</button>

      {/* Renderizado condicional con ternario */}
      {submitted ? (
        <p>Formulario enviado</p>
      ) : (
        <p>Complete y envíe el formulario</p>
      )}
    </form>
  );
};
```

Características:
- Los inputs usan `value` y `onChange` para mantener el estado en React.
- Fácil acceso a los **valores** del formulario (por estado).
- Buen control, pero puede re-renderizar más.

### 10.2 Formulario con React Hook Form (rendimiento y validación)

```tsx
import { useForm } from 'react-hook-form';

type FormInputs = {
  email: string;
  password: string;
};

export const LoginFormRHF = () => {
  const { register, handleSubmit, formState, watch, reset } = useForm<FormInputs>({
    defaultValues: {
      email: 'fernando@google.com',
      password: 'Abc123',
    },
  });

  // Envío y valores
  const onSubmit = (data: FormInputs) => {
    console.log({ data }); // ← Valores del formulario
  };

  // Observar cambios en tiempo real
  const currentEmail = watch('email');

  return (
    <>
      <form onSubmit={handleSubmit(onSubmit)}>
        <h3>Formularios (React Hook Form)</h3>
        <div style={{ display: 'flex', flexDirection: 'column' }}>
          <input
            type="text"
            placeholder="Email"
            {...register('email', { required: 'El email es obligatorio' })}
          />
          <input
            type="password"
            placeholder="Password"
            {...register('password', {
              required: 'El password es obligatorio',
              minLength: { value: 6, message: 'Mínimo 6 caracteres' },
            })}
          />

          <button type="submit">Ingresar</button>
          <button type="button" onClick={() => reset()}>Reset</button>
        </div>
      </form>

      {/* Valores y estado del formulario */}
      <p>Email actual: {currentEmail}</p>
      <pre>{JSON.stringify(formState, null, 2)}</pre>
    </>
  );
};
```

Notas:
- `register` conecta inputs sin necesidad de `onChange` manual.
- `handleSubmit` gestiona el **envío** y pasa los **valores** tipados al `onSubmit`.
- `watch` permite leer valores en tiempo real.
- `formState.errors` contiene errores de validación.

### 10.3 Validaciones comunes

```tsx
<input
  type="email"
  placeholder="Email"
  {...register('email', {
    required: 'El email es obligatorio',
    pattern: {
      value: /[^@\s]+@[^@\s]+\.[^@\s]+/,
      message: 'Email no válido',
    },
  })}
/>
{formState.errors.email && (
  <small style={{ color: 'tomato' }}>
    {formState.errors.email.message}
  </small>
)}
```

### 10.4 Envío a API con axios

```tsx
import axios from 'axios';

const onSubmit = async (data: FormInputs) => {
  try {
    const resp = await axios.post('https://api.ejemplo.com/login', data);
    console.log('Status:', resp.status);
    console.log('Token:', resp.data.token);
  } catch (error) {
    console.log('Error en login');
  }
};
```

### 10.5 Buenas prácticas
- Tipar `FormInputs` con TypeScript.
- Usar `defaultValues` para formularios editables.
- Mostrar feedback de errores cerca de los inputs.
- Evitar re-renderizado excesivo con librerías como React Hook Form.
- Resetear el formulario tras envíos exitosos si aplica.

## 📝 Resumen General Actualizado

### Tipos Básicos
- `string`, `number`, `boolean`
- Arrays: `string[]`, `number[]`
- Operador ternario para condicionales

### Interfaces
- Definen la estructura de objetos
- Propiedades opcionales con `?`
- Se usan en props y respuestas de APIs

### Funciones
- Tipar argumentos: `(a: number, b: number)`
- Tipar retorno: `: string`, `: number`, `: void`
- Argumentos opcionales con `?`
- Valores por defecto

### useState
- Agrega estado a componentes funcionales
- Sintaxis: `const [valor, setValor] = useState(inicial)`
- SIEMPRE usar la función set para modificar
- Crear copias nuevas de objetos/arrays
- Renderizado condicional con ternarios y &&

### useEffect
- Ejecuta código en el ciclo de vida del componente
- Sin dependencias: cada render
- `[]`: solo al montar
- `[var]`: cuando cambia la variable
- Retornar función de limpieza para cleanup

### Custom Hooks
- Reutilizan lógica entre componentes
- DEBEN empezar con "use"
- Pueden usar otros hooks
- Retornan estado y/o funciones
- Ejemplos: useCounter, useForm, useFetch

### Zustand
- Estado global simple y rápido
- No necesita Provider
- Se accede desde cualquier componente
- Estructura: estado inicial + acciones
- Seleccionar solo lo necesario para optimizar

---

### Axios / HTTP
- Peticiones tipadas con generics (`axios.get<T>()`)
- Manejo de `loading`, `error`, `status`
- Interceptores y `baseURL`

### Formularios
- Controlados con `useState` para valores y envío
- React Hook Form: `register`, `handleSubmit`, `watch`, `formState.errors`
- Validaciones y envío con axios

##  Ejercicios Propuestos

1. **Tipos Básicos:** Crea un componente que muestre información de un producto (nombre, precio, disponible)
2. **Interfaces:** Define una interface `Product` y úsala en un componente
3. **Funciones:** Crea una función que calcule el precio con descuento
4. **useState:** Crea un contador que no pueda ser menor a 0
5. **useEffect:** Crea un componente que cargue datos de una API al montar
6. **Custom Hook:** Crea un `useLocalStorage` que guarde y lea del localStorage
7. **Zustand:** Crea un store para un carrito de compras con agregar/eliminar productos
8. **Combinado:** Crea un formulario de login que use Zustand para la autenticación

---

##  Recursos Adicionales

- [Documentación de TypeScript](https://www.typescriptlang.org/docs/)
- [Documentación de React](https://react.dev/)
- [Repositorio de Referencia](https://github.com/Klerith/RN-reforzamiento-react)
