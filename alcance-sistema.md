📄 Documento Técnico

Sistema de Tienda Virtual de Camisetas Personalizables (2D → 3D)

1. Descripción General

1.1 Propósito

Desarrollar una plataforma web de comercio electrónico que permita a los usuarios personalizar camisetas mediante un configurador visual 2D, seleccionando diseños predefinidos y ubicándolos en zonas específicas de la prenda, para posteriormente comprar el producto personalizado.

El sistema deberá ser escalable, permitiendo en futuras fases incorporar:

renderizado 3D

más tipos de prendas

lógica avanzada de impresión

integraciones de pago y producción

1.2 Alcance

El sistema permitirá:

catálogo de camisetas

selección de talla y color

editor visual 2D por zonas

uso exclusivo de diseños precargados (sin uploads del usuario)

carrito de compras

autenticación de usuarios

generación de previews PNG

almacenamiento de diseños en formato JSON

creación de órdenes

panel administrativo básico

No se incluye en el MVP:

pagos reales (puede mockearse)

editor 3D

múltiples tipos de prenda

personalización libre fuera de zonas

1.3 Stack tecnológico

Frontend

Next.js (App Router)

TypeScript

Tailwind

Konva (canvas 2D)

Zustand

Backend

Next.js Route Handlers / Server Actions

Prisma ORM

PostgreSQL

NextAuth

Zod

Infraestructura

Docker

Cloudinary (assets)

Vercel o Railway

2. Requerimientos Funcionales

2.1 Catálogo

El sistema debe permitir:

listar productos disponibles

filtrar por color

visualizar detalle

seleccionar talla

visualizar precio por talla

2.2 Configurador de camiseta (core del sistema)

2.2.1 Selección de vista

El usuario podrá editar:

frontal

trasera

manga izquierda

manga derecha

Cada vista es independiente.

2.2.2 Zonas de impresión

Cada vista contendrá zonas predefinidas.

Ejemplo:

Front:

chest_center

left_chest

full_front

Back:

back_center

upper_back

Sleeves:

sleeve_left

sleeve_right

2.2.3 Restricciones del editor

El sistema debe:

permitir solo diseños del catálogo

restringir movimiento dentro de la zona

permitir:

mover

escalar

rotar

soportar múltiples diseños por zona

permitir eliminar/reordenar capas

2.2.4 Persistencia del diseño

Durante la edición:

guardar estado como JSON

Al agregar al carrito:

generar preview PNG

Al confirmar compra:

generar PNG alta resolución

2.3 Carrito

Debe permitir:

agregar productos personalizados

modificar cantidad

eliminar ítems

recalcular totales

2.4 Órdenes

Debe permitir:

crear orden tras checkout

almacenar:

productos

talla

diseño JSON

preview

estado

Estados:

pending

paid

processing

shipped

2.5 Autenticación

Debe incluir:

registro/login

sesión persistente

historial de órdenes

roles (admin / user)

2.6 Administración

Panel admin debe permitir:

CRUD productos

CRUD diseños

CRUD zonas

ver órdenes

3. Reglas de Negocio

3.1 Precios

el precio depende únicamente de la talla

el diseño no altera el precio

las zonas no alteran el precio

3.2 Diseños

solo assets precargados por el administrador

no se permite upload de usuarios

cada asset tiene dimensiones base

3.3 Zonas

los diseños no pueden salir del bounding box

las zonas son combinables (no exclusivas)

cada zona puede contener múltiples elementos

3.4 Persistencia

el diseño maestro es JSON

el PNG es solo preview/producción

el JSON es la fuente de verdad

3.5 Rendimiento

previews ≤ 600px

impresión ≥ 3000px

máximo recomendado 5 elementos por zona (soft limit)

3.6 Seguridad

endpoints protegidos por auth

validación con Zod

sanitización de inputs

acceso admin restringido

4. Requerimientos No Funcionales

4.1 Performance

Lighthouse ≥ 90

carga inicial ≤ 2s

render canvas fluido

4.2 Escalabilidad

arquitectura modular

separación services/repos

diseño preparado para:

múltiples prendas

3D

microservicios futuros

4.3 Mantenibilidad

TypeScript estricto

ESLint + Prettier

capas desacopladas

pruebas unitarias

4.4 Portabilidad

Docker

DB externa

deploy serverless compatible

5. Modelo de Datos (conceptual)

Entidades principales:

User

Product

Variant (talla/precio)

Zone

DesignAsset

Cart

Order

OrderItem

ItemDesign

Relación clave:

OrderItem
→ múltiples ItemDesign
→ cada uno con zoneId + transformaciones

6. Flujo principal del usuario

Navega catálogo

Selecciona camiseta

Abre configurador

Personaliza zonas

Agrega al carrito

Checkout

Sistema genera preview y orden

7. Fases del proyecto

Fase 1

Backend + DB + API

Fase 2

Catálogo + carrito

Fase 3

Editor 2D

Fase 4

Deploy + optimización

Fase 5 (futuro)

Editor 3D (React Three Fiber)

🎯 Resultado esperado

El sistema final debe comportarse como:
👉 un configurador profesional tipo Printful/Nike By You

Y a nivel técnico demostrar:

Full Stack real

SSR

DB relacional

Canvas avanzado

Arquitectura limpia

Deploy productivo
