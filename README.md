Voy a darte un **README completo orientado a LLMs y desarrolladores**, para que cualquier modelo o persona pueda **retomar el proyecto sin contexto previo**.
Este README explica **qué es el sistema, arquitectura, modelo de datos, flujos, y cómo extenderlo**.

Puedes subirlo como **`README.md` en la raíz del repo**.

---

# E-Taxes Protocol

## Sistema de Seguimiento de Devoluciones de IVA (SAT México)

Prototype / Demo del **E-Taxes Protocol**, un sistema de gestión operativa para el seguimiento de **solicitudes de devolución de IVA ante el SAT** y la coordinación de **respuestas a requerimientos (IDR)**.

El objetivo es convertir un proceso tradicionalmente **caótico, documental y dependiente de correos** en un **sistema estructurado con trazabilidad, asignación de tareas y expediente digital**.

---

# 1. Problema que resuelve

Las devoluciones de IVA en México implican:

1. Presentación de la solicitud
2. Recepción de **requerimientos del SAT (IDR)**
3. Integración de **expedientes complejos**
4. Coordinación entre:

   * equipo fiscal
   * contabilidad
   * tesorería
   * operaciones
   * asesores externos
5. Cumplimiento de **plazos legales estrictos**

En la práctica esto se gestiona con:

* Excel
* correos
* carpetas compartidas
* documentos sueltos

Esto genera:

* pérdida de control de tareas
* vencimientos de plazos
* expedientes incompletos
* respuestas deficientes al SAT

---

# 2. Qué hace el sistema

E-Taxes Protocol crea un **modelo de proyecto para cada devolución de IVA**, con:

### Dashboard ejecutivo

Visualización del portafolio completo de devoluciones.

KPIs:

* IVA total pendiente
* IVA depositado
* IVA en proceso SAT
* IDRs por contestar
* saldos totales

---

### Drill-down operativo

Arquitectura de navegación:

```
Dashboard general
      │
      ▼
Proyecto de devolución (periodo mensual)
      │
      ▼
Requerimiento SAT (IDR)
      │
      ▼
Punto del requerimiento
      │
      ▼
Inciso
      │
      ▼
Tareas
      │
      ▼
Papeles de trabajo
```

---

# 3. Conceptos fiscales clave

### Saldo a favor

Monto de IVA solicitado en devolución.

Ejemplo:

```
Periodo: 2024-09
Monto: $6,830,000
Estado: IDR pendiente
```

---

### IDR (Información Documental Requerida)

Requerimiento emitido por el SAT para:

* verificar materialidad
* validar acreditamientos
* revisar operaciones
* confirmar conciliaciones

Ejemplo:

```
1er requerimiento
Oficio: CFD44
```

---

### Punto del requerimiento

Cada tema solicitado por el SAT.

Ejemplo:

```
1. Integración de proveedores
2. Conciliación bancaria
3. CFDIs emitidos
4. Operaciones con partes relacionadas
```

---

### Inciso

Subdivisión del punto.

Ejemplo:

```
1.a Proveedores principales
1.b Contratos
1.c Pagos bancarios
```

---

### Tarea

Unidad mínima operativa.

Ejemplo:

```
Descargar CFDIs del SAT
Cruzar contra estados de cuenta
Preparar papel de trabajo
```

---

# 4. Roles del sistema

## Administrador

Equipo fiscal / consultor.

Puede:

* ver todos los proyectos
* crear requerimientos
* crear puntos
* asignar responsables
* cambiar estados
* revisar entregables

Entra por:

```
index.html
```

---

## Cliente

Empresa solicitante.

Puede:

* ver el estado del proyecto
* subir documentos
* monitorear avances

Acceso de solo lectura.

---

## Colaborador

Responsable de una tarea específica.

Solo ve:

* su tarea
* fuentes de información
* documentos requeridos
* plazo

Acceso:

```
proyecto.html
```

login → redirige a

```
colaborador.html?colaborador=A
```

Usuarios demo:

```
A → claveA
B → claveB
C → claveC
```

---

# 5. Arquitectura técnica actual

El demo es **100% frontend**.

Tecnologías:

```
HTML
CSS
JavaScript vanilla
GitHub Pages
```

No hay backend todavía.

Todos los datos están simulados en:

```
arrays JS
```

Ejemplo:

```javascript
const T = [
{
 id:"t1",
 period:"2024-09",
 collaborator:"A",
 desc:"Descargar CFDIs proveedores",
 status:"pendiente"
}
]
```

---

# 6. Estructura del proyecto

```
e-taxes-demo
│
├── index.html
│   Dashboard administrador
│
├── proyecto.html
│   Login de colaboradores
│
├── colaborador.html
│   Vista de tareas asignadas
│
└── README.md
```

---

# 7. Flujo del sistema

### 1 Presentación de devolución

Se crea proyecto:

```
2024-09
$6,830,000
```

---

### 2 SAT emite IDR

Ejemplo:

```
1er requerimiento
```

---

### 3 Se crean puntos

Ejemplo:

```
1 CFDIs proveedores
2 Conciliación bancaria
3 Contratos
```

---

### 4 Se crean tareas

Asignadas a:

```
A
B
C
```

---

### 5 Colaboradores integran expediente

Suben:

```
CFDIs
Excels
contratos
estados de cuenta
```

---

### 6 Administrador integra respuesta

Genera expediente final para el SAT.

---

# 8. Estado de tareas

Estados posibles:

```
pendiente
proceso
completo
```

Visualización tipo semáforo:

```
rojo
amarillo
verde
```

---

# 9. Orden de urgencia

Las tareas se ordenan por **fecha límite del requerimiento**.

Mapa en el código:

```
dueDates
```

Ejemplo:

```
2024-07 → 1 Oct
2024-08 → 1 Nov
```

Esto permite mostrar:

```
tareas más urgentes primero
```

---

# 10. Limitaciones actuales

Este repositorio es **solo un prototipo visual**.

No tiene:

* base de datos
* autenticación real
* almacenamiento de archivos
* API

---

# 11. Arquitectura futura

El sistema está diseñado para evolucionar a:

```
Frontend
   ↓
API
   ↓
Base de datos
   ↓
Repositorio de documentos
```

Tecnologías sugeridas:

Backend:

```
Node
Python
Go
```

Base de datos:

```
PostgreSQL
Firestore
MongoDB
```

Storage:

```
S3
GCS
Azure Blob
```

---

# 12. Integración con TaxFlow

El sistema puede conectarse con pipelines de extracción fiscal:

```
SAT metadata
CFDI XML
DIOT
contabilidad
bancos
```

Para generar automáticamente:

```
cruces
conciliaciones
expedientes
```

---

# 13. Objetivo del proyecto

Construir el **primer sistema operativo para devoluciones de IVA en México**.

No solo:

```
ver estados
```

sino **operar el expediente completo**.

---

# 14. Estado del proyecto

Fase actual:

```
Demo conceptual
```

Funcional:

* dashboard
* drill-down
* tareas
* vista colaborador

---

# 15. Roadmap

### Fase 1

Demo funcional

### Fase 2

Persistencia de datos

### Fase 3

Gestión documental

### Fase 4

Integración SAT

### Fase 5

Automatización con IA

---

# 16. Licencia

Demo interno / experimental.

---

# 17. Autor

Concepto y diseño:

César Castro López

E-Taxes Protocol

---

Si quieres, en el siguiente paso puedo también generarte:

1️⃣ **README versión "LLM-optimized"** (para que cualquier modelo continúe el desarrollo automáticamente)
2️⃣ **arquitectura técnica formal del sistema**
3️⃣ **modelo de base de datos**
4️⃣ **prompt maestro para que LLMs continúen el proyecto**

Eso hará que **cualquier IA pueda retomar el proyecto sin explicaciones humanas**.
