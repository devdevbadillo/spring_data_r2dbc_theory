# Spring Data R2DBC

## Tabla de Contenido

- [Introducción a R2DBC y Spring Data R2DBC](#introduccion-r2dbc-spring-data)
  - [El Problema de JDBC en Entornos Reactivos](#problema-jdbc-reactivos)
  - [¿Qué es R2DBC?](#que-es-r2dbc)
    - [Especificación R2DBC](#especificacion-r2dbc)
    - [Controladores (Drivers) R2DBC](#controladores-r2dbc)
    - [Principios Clave (No Bloqueante, Reactivo, Asíncrono)](#principios-clave-r2dbc)
  - [¿Qué es Spring Data R2DBC?](#que-es-spring-data-r2dbc)
    - [Propósito y Arquitectura](#proposito-arquitectura-spring-data-r2dbc)
    - [Ventajas de Usar Spring Data R2DBC](#ventajas-spring-data-r2dbc)
  - [Configuración Inicial del Proyecto](#configuracion-inicial-r2dbc)
    - [Dependencias (Maven/Gradle)](#dependencias-r2dbc)
    - [Configuración de Conexión (`application.properties/yml`)](#configuracion-conexion-r2dbc)
    - [Habilitar Spring Data R2DBC](#habilitar-spring-data-r2dbc)

- [Mapeo de Entidades (Mapping)](#mapeo-entidades-r2dbc-sd)
  - [Entidades y Tablas](#entidades-tablas-r2dbc)
    - [`@Table`](#table-annotation-r2dbc)
    - [`@Id` y Generación de IDs](#id-generacion-ids-r2dbc)
    - [`@Column`](#column-annotation-r2dbc)
    - [`@Transient`](#transient-annotation-r2dbc)
  - [Tipos de Datos Soportados](#tipos-datos-soportados-r2dbc)
  - [Manejo de Enums](#manejo-enums-r2dbc)
  - [Relaciones (Embeddable & Referencias)](#relaciones-r2dbc)
    - [Objetos Embebidos (`@Embedded`)](#objetos-embebidos-r2dbc)
    - [Manejo de Relaciones (Uno a Uno, Uno a Muchos, Muchos a Muchos)](#manejo-relaciones-r2dbc)
      - [Enfoque Reactivo para Relaciones (composición de flujos)](#enfoque-reactivo-relaciones)
  - [Mapeo Personalizado](#mapeo-personalizado-r2dbc)
    - [`R2dbcCustomConversions`](#r2dbccustomconversions)
    - [`MappingR2dbcConverter`](#mappingr2dbccoverter)

- [Repositorios de Spring Data R2DBC](#repositorios-spring-data-r2dbc-sd)
  - [`ReactiveCrudRepository`](#reactivecrudrepository-r2dbc)
    - [Métodos CRUD Reactivos Estándar](#metodos-crud-reactivos-estandar)
    - [Definición de Consultas Derivadas de Métodos (Query Methods)](#consultas-derivadas-metodos-r2dbc)
      - [Convenciones de Nomenclatura](#convenciones-nomenclatura-queries-r2dbc)
      - [Palabras Clave Soportadas](#palabras-clave-soportadas-r2dbc)
  - [`@Query` Anotación](#query-annotation-r2dbc)
    - [Consultas SQL Nativas Directas](#consultas-sql-nativas-r2dbc)
    - [Uso de SpEL (Spring Expression Language)](#spel-query-r2dbc)
    - [Proyección y Ordenación](#proyeccion-ordenacion-query-r2dbc)
  - [Paginación y Ordenación Reactiva](#paginacion-ordenacion-reactiva)
    - [`Pageable` Reactivo](#pageable-reactivo)
    - [`Sort` Reactivo](#sort-reactivo)
  - [Manejo de Errores Reactivos en Repositorios](#manejo-errores-repositorios-r2dbc)

- [R2dbcEntityTemplate](#r2dbcentitytemplate)
  - [¿Cuándo usar `R2dbcEntityTemplate` vs. Repositorios?](#cuando-usar-r2dbcentitytemplate-vs-repositorios)
  - [Operaciones Básicas Reactivas](#operaciones-basicas-r2dbcentitytemplate)
    - [`insert()`, `update()`, `delete()`, `select()`](#metodos-crud-r2dbcentitytemplate)
  - [Consultas Avanzadas con `Query` y `Criteria`](#consultas-avanzadas-r2dbcentitytemplate)
    - [`Query` y `Criteria` Builder](#query-criteria-builder-r2dbcentitytemplate)
    - [Operadores de Consulta](#operadores-consulta-r2dbcentitytemplate)
    - [Combinación de Criterios](#combinacion-criterios-r2dbcentitytemplate)
  - [Proyección, Ordenación, Paginación](#proyeccion-ordenacion-paginacion-r2dbcentitytemplate)
  - [Manejo de Errores en `R2dbcEntityTemplate`](#manejo-errores-r2dbcentitytemplate)

- [Transacciones Reactivas](#transacciones-reactivas-r2dbc)
  - [Concepto de Transacciones Reactivas](#concepto-transacciones-reactivas)
  - [`ReactiveTransactionManager`](#reactivetransactionmanager)
  - [`@Transactional` en Entornos WebFlux](#transactional-webflux-r2dbc)
  - [`TransactionalOperator` (Transacciones Programáticas)](#transactionaloperator-r2dbc)
  - [Consideraciones sobre Aislamiento y Propagación](#aislamiento-propagacion-r2dbc)
  - [Manejo de Errores y Rollbacks en Transacciones Reactivas](#errores-rollbacks-transacciones-r2dbc)

- [Características Avanzadas y Optimizaciones](#caracteristicas-avanzadas-optimizaciones-r2dbc)
  - [Manejo de Índices](#manejo-indices-r2dbc)
    - [Creación de Índices](#creacion-indices-r2dbc)
    - [Consideraciones de Rendimiento](#consideraciones-rendimiento-indices-r2dbc)
  - [Auditoría](#auditoria-r2dbc)
    - [`@EnableR2dbcAuditing`](#enabler2dbcauditing)
    - [`@CreatedDate`, `@LastModifiedDate`, `@CreatedBy`, `@LastModifiedBy`](#auditoria-annotations-r2dbc)
  - [Ciclo de Vida de las Entidades (Callbacks)](#ciclo-vida-entidades-r2dbc)
    - [`@EventListener` con eventos de Spring Data R2DBC](#eventlistener-r2dbc)
    - [`@PrePersist`, `@PostPersist`, etc.](#callbacks-annotations-r2dbc)
  - [Integración con Spring Boot Actuator](#integracion-spring-boot-actuator-r2dbc)
    - [Health Indicators](#health-indicators-r2dbc)
    - [Métricas](#metricas-r2dbc)

- [Testing en Spring Data R2DBC](#testing-spring-data-r2dbc)
  - [Pruebas Unitarias de Repositorios y Servicios](#pruebas-unitarias-r2dbc)
  - [Pruebas de Integración con Bases de Datos en Memoria (ej. H2 R2DBC)](#pruebas-integracion-r2dbc-inmemory)
  - [Testcontainers para Bases de Datos Relacionales](#testcontainers-r2dbc)
  - [Estrategias de Limpieza de Datos](#limpieza-datos-testing-r2dbc)

<a id="introduccion-r2dbc-spring-data"></a>
## Introducción a R2DBC y Spring Data R2DBC

<a id="problema-jdbc-reactivos"></a>
### El Problema de JDBC en Entornos Reactivos
<a id="que-es-r2dbc"></a>
### ¿Qué es R2DBC?
<a id="especificacion-r2dbc"></a>
#### Especificación R2DBC
<a id="controladores-r2dbc"></a>
#### Controladores (Drivers) R2DBC
<a id="principios-clave-r2dbc"></a>
#### Principios Clave (No Bloqueante, Reactivo, Asíncrono)
<a id="que-es-spring-data-r2dbc"></a>
### ¿Qué es Spring Data R2DBC?
<a id="proposito-arquitectura-spring-data-r2dbc"></a>
#### Propósito y Arquitectura
<a id="ventajas-spring-data-r2dbc"></a>
#### Ventajas de Usar Spring Data R2DBC
<a id="configuracion-inicial-r2dbc"></a>
### Configuración Inicial del Proyecto
<a id="dependencias-r2dbc"></a>
#### Dependencias (Maven/Gradle)
<a id="configuracion-conexion-r2dbc"></a>
#### Configuración de Conexión (`application.properties/yml`)
<a id="habilitar-spring-data-r2dbc"></a>
#### Habilitar Spring Data R2DBC

<a id="mapeo-entidades-r2dbc-sd"></a>
## Mapeo de Entidades (Mapping)

<a id="entidades-tablas-r2dbc"></a>
### Entidades y Tablas
<a id="table-annotation-r2dbc"></a>
#### `@Table`
<a id="id-generacion-ids-r2dbc"></a>
#### `@Id` y Generación de IDs
<a id="column-annotation-r2dbc"></a>
#### `@Column`
<a id="transient-annotation-r2dbc"></a>
#### `@Transient`
<a id="tipos-datos-soportados-r2dbc"></a>
### Tipos de Datos Soportados
<a id="manejo-enums-r2dbc"></a>
### Manejo de Enums
<a id="relaciones-r2dbc"></a>
### Relaciones (Embeddable & Referencias)
<a id="objetos-embebidos-r2dbc"></a>
#### Objetos Embebidos (`@Embedded`)
<a id="manejo-relaciones-r2dbc"></a>
#### Manejo de Relaciones (Uno a Uno, Uno a Muchos, Muchos a Muchos)
<a id="enfoque-reactivo-relaciones"></a>
##### Enfoque Reactivo para Relaciones (composición de flujos)
<a id="mapeo-personalizado-r2dbc"></a>
### Mapeo Personalizado
<a id="r2dbccustomconversions"></a>
#### `R2dbcCustomConversions`
<a id="mappingr2dbccoverter"></a>
#### `MappingR2dbcConverter`

<a id="repositorios-spring-data-r2dbc-sd"></a>
## Repositorios de Spring Data R2DBC

<a id="reactivecrudrepository-r2dbc"></a>
### `ReactiveCrudRepository`
<a id="metodos-crud-reactivos-estandar"></a>
#### Métodos CRUD Reactivos Estándar
<a id="consultas-derivadas-metodos-r2dbc"></a>
#### Definición de Consultas Derivadas de Métodos (Query Methods)
<a id="convenciones-nomenclatura-queries-r2dbc"></a>
##### Convenciones de Nomenclatura
<a id="palabras-clave-soportadas-r2dbc"></a>
##### Palabras Clave Soportadas
<a id="query-annotation-r2dbc"></a>
### `@Query` Anotación
<a id="consultas-sql-nativas-r2dbc"></a>
#### Consultas SQL Nativas Directas
<a id="spel-query-r2dbc"></a>
#### Uso de SpEL (Spring Expression Language)
<a id="proyeccion-ordenacion-query-r2dbc"></a>
#### Proyección y Ordenación
<a id="paginacion-ordenacion-reactiva"></a>
### Paginación y Ordenación Reactiva
<a id="pageable-reactivo"></a>
#### `Pageable` Reactivo
<a id="sort-reactivo"></a>
#### `Sort` Reactivo
<a id="manejo-errores-repositorios-r2dbc"></a>
### Manejo de Errores Reactivos en Repositorios

<a id="r2dbcentitytemplate"></a>
## R2dbcEntityTemplate

<a id="cuando-usar-r2dbcentitytemplate-vs-repositorios"></a>
### ¿Cuándo usar `R2dbcEntityTemplate` vs. Repositorios?
<a id="operaciones-basicas-r2dbcentitytemplate"></a>
### Operaciones Básicas Reactivas
<a id="metodos-crud-r2dbcentitytemplate"></a>
#### `insert()`, `update()`, `delete()`, `select()`
<a id="consultas-avanzadas-r2dbcentitytemplate"></a>
### Consultas Avanzadas con `Query` y `Criteria`
<a id="query-criteria-builder-r2dbcentitytemplate"></a>
#### `Query` y `Criteria` Builder
<a id="operadores-consulta-r2dbcentitytemplate"></a>
#### Operadores de Consulta
<a id="combinacion-criterios-r2dbcentitytemplate"></a>
#### Combinación de Criterios
<a id="proyeccion-ordenacion-paginacion-r2dbcentitytemplate"></a>
### Proyección, Ordenación, Paginación
<a id="manejo-errores-r2dbcentitytemplate"></a>
### Manejo de Errores en `R2dbcEntityTemplate`

<a id="transacciones-reactivas-r2dbc"></a>
## Transacciones Reactivas

<a id="concepto-transacciones-reactivas"></a>
### Concepto de Transacciones Reactivas
<a id="reactivetransactionmanager"></a>
### `ReactiveTransactionManager`
<a id="transactional-webflux-r2dbc"></a>
### `@Transactional` en Entornos WebFlux
<a id="transactionaloperator-r2dbc"></a>
### `TransactionalOperator` (Transacciones Programáticas)
<a id="aislamiento-propagacion-r2dbc"></a>
### Consideraciones sobre Aislamiento y Propagación
<a id="errores-rollbacks-transacciones-r2dbc"></a>
### Manejo de Errores y Rollbacks en Transacciones Reactivas

<a id="caracteristicas-avanzadas-optimizaciones-r2dbc"></a>
## Características Avanzadas y Optimizaciones

<a id="manejo-indices-r2dbc"></a>
### Manejo de Índices
<a id="creacion-indices-r2dbc"></a>
#### Creación de Índices
<a id="consideraciones-rendimiento-indices-r2dbc"></a>
#### Consideraciones de Rendimiento
<a id="auditoria-r2dbc"></a>
### Auditoría
<a id="enabler2dbcauditing"></a>
#### `@EnableR2dbcAuditing`
<a id="auditoria-annotations-r2dbc"></a>
#### `@CreatedDate`, `@LastModifiedDate`, `@CreatedBy`, `@LastModifiedBy`
<a id="ciclo-vida-entidades-r2dbc"></a>
### Ciclo de Vida de las Entidades (Callbacks)
<a id="eventlistener-r2dbc"></a>
#### `@EventListener` con eventos de Spring Data R2DBC
<a id="callbacks-annotations-r2dbc"></a>
#### `@PrePersist`, `@PostPersist`, etc.
<a id="integracion-spring-boot-actuator-r2dbc"></a>
### Integración con Spring Boot Actuator
<a id="health-indicators-r2dbc"></a>
#### Health Indicators
<a id="metricas-r2dbc"></a>
#### Métricas

<a id="testing-spring-data-r2dbc"></a>
## Testing en Spring Data R2DBC

<a id="pruebas-unitarias-r2dbc"></a>
### Pruebas Unitarias de Repositorios y Servicios
<a id="pruebas-integracion-r2dbc-inmemory"></a>
### Pruebas de Integración con Bases de Datos en Memoria (ej. H2 R2DBC)
<a id="testcontainers-r2dbc"></a>
### Testcontainers para Bases de Datos Relacionales
<a id="limpieza-datos-testing-r2dbc"></a>
### Estrategias de Limpieza de Datos
