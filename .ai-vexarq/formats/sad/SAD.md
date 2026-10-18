# Documento de arquitectura de software

## Control de cambios

| Versión | Fecha | Descripción |
|---|---|---|
| 0.1.0 |  | Borrador preliminar |

- Estado: preliminar | vigente
- Insumos vigentes: SRS y diseño de datos
- Perfil:

## 1. Introducción

### 1.1 Propósito

Qué decisiones fija este documento y qué implementación debe obedecerlo.

### 1.2 Alcance

Qué se construye y qué queda fuera.

### 1.3 Contexto y stack

Stack confirmado por el repositorio o por el usuario. Las alternativas no elegidas van a decisiones pendientes.

| Pieza | Elección | Estado | Motivo |
|---|---|---|---|

## 2. Vista de contexto

Diagrama C4 de contexto en Mermaid: usuarios, sistema y externos reales.

## 3. Contenedores y patrones

Estilo elegido (monolito modular, capas, hexagonal u otro) y por qué cabe en este problema.
Contenedores: aplicación, datos, interfaz y externos. Sin nodos de relleno.

## 4. Decisiones

| Id | Contexto | Decisión | Consecuencia | Descartada |
|---|---|---|---|---|

## 5. Calidad

Atributos observables y no observables que el SRS pidió. Conflictos entre atributos, si existen.

## 6. Seguridad

Identidad, autorización, secretos, validación y superficie de ataque del alcance real. En perfil crítico, ampliar. En un sitio estático, no simular un IAM.

## 7. Datos

Cómo la arquitectura usa el diseño de datos vigente. Cambios necesarios quedan como decisión pendiente, no como un esquema nuevo.

## 8. Procesos centrales

Para cada proceso que el SRS marca como principal:

- Objetivo.
- Componentes.
- Secuencia.
- Validaciones de negocio.
- Fallo esperado.

## 9. Despliegue y operación

Entornos, configuración, migración, observación y recuperación, al tamaño del perfil. Comandos concretos solo si el repositorio ya los tiene o el arquetipo posterior los creará.

## 10. Riesgos y huecos

Qué no está decidido y qué no debe implementar el arquetipo hasta que exista un vigente nuevo.
