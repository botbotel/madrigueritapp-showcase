# MadrigueritApp

🇬🇧 [English](../README.md) | 🇪🇸 **Español**

**Producto multiplataforma para la gestión compartida del hogar, construido con React Native, Expo, TypeScript y Firebase.**

> **Repositorio de portfolio.**  
> El código fuente de producción se mantiene privado de forma intencionada. Este repositorio documenta el producto, la arquitectura y las decisiones de ingeniería sin exponer código reutilizable ni la implementación privada.

MadrigueritApp es una aplicación de gestión doméstica compartida diseñada para **iOS, Android y Web**. Reúne flujos cotidianos como tareas, calendario, lista de la compra y gastos dentro de un modelo multi-tenant por hogar, con control de acceso por roles y sincronización en tiempo real.

---

## Visión del producto

El objetivo es reducir la fricción de coordinar un hogar compartido sin repartir la información entre chats, notas, calendarios y hojas de cálculo.

La aplicación centraliza:

- tareas y tickets compartidos
- calendario y elementos programados
- lista de la compra en tiempo real
- gastos del hogar
- miembros y roles
- acceso mediante códigos de invitación
- gestión de perfil y permisos

El proyecto está planteado como un producto real y no como una simple demo de interfaz: autenticación, autorización, modelo de datos, estado, navegación, validación y reglas de backend forman parte de la arquitectura.

---

## Stack principal

| Área | Tecnología |
| --- | --- |
| Web y móvil | React 19, React Native, Expo |
| Routing | Expo Router |
| Lenguaje | TypeScript |
| Estado | Zustand |
| Validación | Zod |
| Backend | Firebase Authentication, Cloud Firestore |
| Tiempo real | Firestore listeners |
| Seguridad | Firestore Security Rules |
| Builds | EAS Build |
| Despliegue web | Expo Web + Firebase Hosting |

---

## Áreas principales del producto

### Autenticación y navegación protegida

La aplicación utiliza Firebase Authentication, rutas protegidas y persistencia de sesión en entornos web y nativos.

### Hogares multi-tenant

Los usuarios trabajan dentro de un hogar activo en lugar de utilizar un modelo de datos individual.

El producto incluye:

- creación de hogares
- roles owner / member / guest
- códigos de invitación
- unión a hogares existentes
- estado de membresía
- gestión de miembros
- acciones restringidas al propietario

### Tickets

Gestión compartida de tareas con:

- título y descripción
- prioridad
- estado
- fecha límite
- asignación
- información del creador
- actualizaciones en tiempo real

### Calendario

El calendario combina distintos dominios del producto en una única vista:

- eventos del hogar
- tickets con fecha límite

De esta forma se evita duplicar la misma información en una colección independiente de calendario.

### Compra

Lista de la compra compartida y sincronizada en tiempo real con:

- creación de elementos
- notas opcionales
- estado completado
- selección y acciones en lote
- actualizaciones en vivo entre miembros del hogar

### Gastos

Gestión de gastos asociada al hogar con:

- categorías
- importes
- fechas
- notas
- agrupación mensual
- validación
- flujos de creación y edición

---

## Arquitectura

La aplicación separa de forma clara el routing, las funcionalidades de producto, los servicios de dominio, el estado y los componentes compartidos.

```text
Expo Router / React Native UI
            │
            ▼
 Hooks y lógica de features
            │
            ▼
       Capa de servicios
            │
      ┌─────┴─────┐
      ▼           ▼
Firebase Auth   Firestore
                  │
                  ▼
          Security Rules
```

La explicación técnica detallada está disponible en [architecture.md](architecture.md) en inglés.

---

## Modelo multi-tenant

La frontera principal de tenancy es el **hogar**.

Los tickets, eventos, elementos de compra y gastos pertenecen a un hogar, no directamente a un usuario individual.

A nivel conceptual:

```text
Usuario
  │
  ▼
Membresía del hogar
  │
  ├── rol
  ├── estado
  └── permisos
  │
  ▼
Hogar
  ├── Tickets
  ├── Eventos
  ├── Compra
  └── Gastos
```

Este diseño simplifica la colaboración, los permisos y la evolución futura del producto.

---

## Enfoque de seguridad

La seguridad no se implementa únicamente en la interfaz.

La autorización también se aplica en la capa de reglas del backend mediante conceptos como:

- comprobación de usuario autenticado
- membresía activa del hogar
- operaciones restringidas al propietario
- validación de roles
- aislamiento entre tenants
- relaciones de tenant inmutables
- restricciones sobre campos modificables
- validación de datos
- política de denegación por defecto

Las reglas exactas de producción no se publican en este repositorio de showcase.

---

## Colaboración en tiempo real

Varias áreas del producto utilizan listeners de Firestore para propagar cambios sin necesidad de refrescar manualmente.

Esto resulta especialmente útil en:

- lista de la compra
- tareas
- eventos
- miembros del hogar
- gastos

El objetivo es que el producto se comporte de forma natural cuando varias personas interactúan con los mismos datos.

---

## Decisiones cross-platform

La aplicación está orientada a:

- **Android**
- **iOS**
- **Web**

Algunos flujos necesitan un tratamiento específico según plataforma, especialmente en navegación, confirmaciones y persistencia de autenticación.

El proyecto incluye además builds de preview y configuración de producción mediante EAS.

---

## Decisiones de ingeniería

### 1. El hogar como frontera de tenant

Los datos compartidos pertenecen al hogar, no a usuarios individuales. Esto mantiene coherentes la colaboración y la autorización.

### 2. Autorización más allá de la interfaz

Deshabilitar un botón no se considera una medida de seguridad. Las restricciones sensibles también se aplican en la capa de datos.

### 3. Servicios organizados por dominio

El acceso a datos se separa por dominio de producto en lugar de integrarse directamente dentro de las pantallas.

Esto mantiene el código de UI centrado en interacción y presentación y aísla las responsabilidades específicas del backend.

### 4. Flujos atómicos entre documentos

Procesos como el alta de un hogar afectan a varios documentos relacionados. Estos flujos están diseñados para conservar la consistencia cuando deben producirse varios cambios de forma conjunta.

### 5. Datos de calendario derivados

Las entradas del calendario se generan a partir de eventos existentes y tickets con fecha, en lugar de mantener una fuente de datos duplicada.

---

## Estado actual

El código privado actual incluye bases funcionales para:

- autenticación
- rutas protegidas
- onboarding de hogares
- membresía basada en roles
- invitaciones
- gestión de tickets
- calendario y eventos
- lista de la compra
- gastos
- ajustes de perfil
- builds web y móvil

El proyecto continúa en desarrollo activo.

---

## Capturas

Las capturas del producto se añadirán progresivamente.

Vistas previstas para el showcase:

1. Inicio / panel del hogar
2. Tickets
3. Calendario
4. Compra
5. Gastos
6. Ajustes del hogar

---

## Qué demuestra este proyecto

Desde el punto de vista de portfolio, MadrigueritApp demuestra experiencia en:

- desarrollo de producto con React Native y Expo
- arquitectura de aplicaciones TypeScript
- interfaces cross-platform
- autenticación y routing protegido
- modelado multi-tenant
- autorización basada en roles
- sincronización de datos en tiempo real
- seguridad de backend
- gestión de estado
- separación por dominios
- decisiones de ingeniería orientadas a producto

---

## Código fuente

El código fuente de MadrigueritApp es **privado por diseño**.

Este repositorio funciona como caso de estudio público para mostrar el producto, la arquitectura y las decisiones técnicas, manteniendo protegidas tanto la implementación como su posible valor comercial.

---

## Autor

**Alberto Botella Guirado**  
Frontend / Product Engineer

[LinkedIn](https://www.linkedin.com/in/alberto-botella-guirado/)
