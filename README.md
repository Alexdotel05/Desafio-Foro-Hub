# 📚 ForoHub - Infraestructura de Comunicación Técnica

¡Bienvenido a **ForoHub**! Este proyecto representa la culminación de un desafío técnico de alto nivel: la creación de una **API REST** robusta que simula el ecosistema de un foro de discusión profesional. No es solo un sistema de mensajes; es una plataforma diseñada bajo los principios de la arquitectura **Stateless**, implementando seguridad avanzada y una gestión de datos relacional optimizada.

Con ForoHub, he buscado materializar los pilares del desarrollo backend moderno: **integridad referencial**, seguridad mediante **JWT** y una lógica de negocio que asegura que cada interacción sea precisa, segura y escalable.

---

## 🚀 Desglose Detallado de Funcionalidades
La lógica de **ForoHub** ha sido diseñada para ofrecer un control total sobre el flujo de información, garantizando que el conocimiento se organice de forma eficiente:

### 1. Sistema de Autenticación y Seguridad Blindada
El acceso a la información está protegido por un motor de seguridad basado en **Spring Security**:
* **Autenticación JWT:** Implementación de tokens con firma digital para sesiones seguras y sin estado.
* **Gestión de Perfiles:** Diferenciación de permisos entre autores y administradores, asegurando que cada usuario acceda solo a lo que le corresponde.

### 2. Gestión Inteligente de Tópicos (CRUD)
Más allá de un registro básico, el sistema implementa:
* **Validación de Integridad:** Prevención automática de duplicados para evitar el "spam" de preguntas idénticas.
* **Mapeo Relacional:** Cada tópico nace vinculado a un autor, un curso y una categoría, asegurando una **trazabilidad total**.

### 3. El "Efecto Dominó": Lógica de Solución
Esta es una de las funciones más sofisticadas del proyecto. Cuando una respuesta es marcada como solución:
* **Persistencia Atómica:** La respuesta cambia su estado a `solucion = true`.
* **Transición de Estado:** Automáticamente, el tópico padre actualiza su estatus a `SOLUCIONADO`, cerrando el ciclo de vida de la duda técnica de forma coherente.

### 4. Borrado Lógico (Preservación de Datos)
En ForoHub, la información es valiosa. Por ello, la eliminación de tópicos no destruye el registro físico:
* **Flag de Actividad:** Se implementó una lógica de `activo = false`. Esto permite mantener la integridad de las estadísticas y el historial del foro, ocultando el contenido de la vista pública sin perder la base de conocimientos.

### 5. Motor de Búsqueda y Filtrado Dinámico
Implementación de consultas personalizadas en **JPA** que permiten segmentar la información por:
* **Nombre del Curso:** Ideal para comunidades de aprendizaje específicas.
* **Año de Creación:** Facilitando el análisis de las dudas más recurrentes en periodos de tiempo determinados.

---

## 🏗️ Arquitectura y Estructura Técnica
El proyecto sigue una estructura de capas limpia (**Clean Architecture**), facilitando el mantenimiento y la implementación de nuevas funcionalidades:

```plaintext
forohub/
├── src/main/java/com/aluracursos/forohub/
│   ├── controller/      # Endpoints de la API (Tópicos, Respuestas, Login, etc.)
│   ├── domain/          # Entidades JPA, DTOs (Records) y Lógica de Negocio
│   │   ├── topico/      # Modelado y reglas de Tópicos
│   │   ├── respuesta/   # Modelado y reglas de Respuestas
│   │   ├── usuario/     # Gestión de usuarios y perfiles
│   │   ├── perfil/      # Entidad Perfil (Roles)
│   │   └── curso/       # Catálogo de cursos
│   ├── infra/           # Infraestructura y Configuraciones Transversales
│   │   ├── security/    # Filtros JWT y Seguridad
│   │   ├── springdoc/   # Configuración de Swagger (OpenAPI 3)
│   │   └── errores/     # Manejo global de excepciones (TratamientoDeErrores)
│   └── ForoHubApplication.java
└── src/main/resources/
    ├── db/migration/    # Scripts de Flyway (Versionamiento de DB)
    └── application.properties
```

## ⚙️ Tecnologías Utilizadas

| Tecnología | Descripción |
| :--- | :--- |
| ☕ **Java 17** | Lenguaje principal con uso de Records para DTOs |
| 🌱 **Spring Boot 4.0.2** | Framework para inicialización y configuración |
| 🔒 **Spring Security** | Protección de rutas y gestión de autenticación |
| 🎟️ **Auth0 JWT** | Generación y validación de tokens de seguridad |
| 🗄️ **Spring Data JPA** | Persistencia y acceso a datos mediante Hibernate |
| 🐬 **MySQL 8.0** | Sistema de base de datos relacional |
| 🧰 **Maven** | Gestión de dependencias y ciclo de compilación |
| 📜 **Flyway** | Control de versiones de la base de datos |
| 📖 **SpringDoc OpenAPI** | Documentación interactiva (Swagger UI) |

---

## 🧠 Conceptos de Programación y Decisiones Técnicas

* **Inmutabilidad con Java Records:** El uso de **Records** para los DTOs asegura que los datos que viajan entre el cliente y el servidor sean íntegros y no puedan ser alterados durante el proceso de transporte.
* **Documentación Viva con Swagger:** Implementé **SpringDoc OpenAPI** para ofrecer una interfaz interactiva donde la API puede ser testeada en tiempo real, facilitando la integración con posibles frontends.
* **Versionamiento de Base de Datos con Flyway:** Cada cambio en el esquema (como la adición del campo `activo` o la tabla de respuestas) fue gestionado mediante migraciones, permitiendo un despliegue controlado y profesional.
* **Manejo de Errores Custom:** Se diseñó la clase `TratamientoDeErrores` para capturar excepciones de validación (400) o recursos no encontrados (404), devolviendo mensajes claros y útiles al usuario.

---
## 🚀 Cómo ejecutar el proyecto

### 1. Clonar el repositorio
Abre tu terminal y ejecuta el siguiente comando:

```bash
git clone https://github.com/Natalia-Schwindt/challenge-foro-hub.git
```

### 2. Configurar las variables de entorno
Para proteger tus datos y seguir las mejores prácticas de seguridad, configura las siguientes variables en tu sistema o directamente en **IntelliJ IDEA**:

* **`DB_NAME3`**: Nombre identificador de la aplicación.
* **`DB_USERNAME`**: Tu usuario de la base de datos MySQL.
* **`DB_PASSWORD`**: Tu contraseña de la base de datos.
* **`JWT_SECRET`**: Una clave secreta (mínimo 32 caracteres recomendados) para firmar tus tokens.

### 3. Configurar el archivo `application.properties`
Asegúrate de que el archivo ubicado en `src/main/resources/application.properties` contenga la siguiente configuración para mapear las variables de entorno:

```properties
spring.application.name=${DB_NAME3}

spring.datasource.url=jdbc:mysql://localhost:3306/forohub_db
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}

spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=none

spring.flyway.enabled=true

api.security.secret=${JWT_SECRET}
```

### 4. Ejecutar la aplicación
Para poner en marcha el sistema, sigue estos pasos finales:

1. **Base de Datos:** Asegúrate de que tu servidor MySQL esté encendido y que hayas creado una base de datos llamada `forohub_db`.
2. **Clase Principal:** Ejecuta la aplicación iniciando la clase `ForoHubApplication.java` desde tu IDE.
3. **Migraciones:** Al iniciar, **Flyway** detectará automáticamente los scripts en `src/main/resources/db/migration` y creará la estructura de tablas necesaria.

---

## 🤝 Agradecimientos
Este proyecto es el resultado de un camino de aprendizaje constante apoyado por:

* **Oracle Next Education (ONE) & Alura Latam:** Por el desafío técnico y la estructura educativa de excelencia que brindan.

---

## 🪐 Licencia

Este proyecto está bajo la **Licencia MIT**. Consulta el archivo [LICENSE](./LICENSE) para más detalles.
