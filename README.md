## 1.0 Descrición do proxecto
EduManager es una aplicación web de gestión académica integral diseñada para centralizar la administración de centros educativos de formación profesional. La herramienta permite gestionar el ciclo de vida completo del alumno: desde la solicitud de matrícula hasta la consulta de horarios y obtención de resúmenes académicos, diferenciando claramente los roles de Administrador, Profesor y Alumno.

## 1.1. Xustificación do proxecto
La idea surge de la necesidad de simplificar los procesos en centros de FP, donde la asignación de módulos, profesores y la validación de matrículas suelen gestionarse de forma fragmentada. El proyecto busca resolver la falta de comunicación directa entre el sistema de matriculación y el horario del alumno, ofreciendo una plataforma única, accesible desde el navegador y con generación automática de documentación en PDF.

## 1.2. Funcionalidades do proxecto
Gestión de Usuarios: Registro, login seguro (JWT) y asignación de roles.

Administración Académica: Creación y mantenimiento de Ciclos, Cursos y Módulos.

Sistema de Matrícula: Flujo de solicitud por parte del alumno y validación/estado por parte del administrador.

Gestión de Horarios: Asignación de franjas horarias a módulos y profesores, con visualización filtrada por usuario.

Generador de Documentación: Creación dinámica de resúmenes de matrícula en formato PDF.

Panel de Control (Dashboard): Vistas personalizadas según el rol para una navegación intuitiva.

## 1.3. Estudo de necesidades
Existen soluciones como Moodle (enfocado a e-learning) o Sallenet/Alexia (gestión escolar integral). Sin embargo, muchas son complejas o requieren infraestructuras pesadas. EduManager se posiciona como una alternativa "Lightweight" y específica para DAM/DAW, eliminando la curva de aprendizaje excesiva y centrándose en la agilidad administrativa y la portabilidad gracias al uso de SQLite.

## 1.4. Persoas destinatarias
El público objetivo son centros de formación profesional, academias privadas o instituciones educativas que requieran una herramienta de gestión interna. Los usuarios finales son:

Personal administrativo (gestión de datos).

Cuerpo docente (consulta de carga lectiva y horarios).

Alumnado (trámites y consultas).

## 1.5. Modelo de negocio
El modelo elegido es In-house / Desarrollo a medida. Se plantea como un software robusto para ser desplegado en los servidores propios del centro educativo, garantizando la privacidad de los datos y la soberanía tecnológica de la institución.

## 2.0 Requirimentos (Stack Tecnolóxico)
Para asegurar un desarrollo moderno y escalable, se utilizarán:

Frontend: React (JavaScript/TypeScript) con Tailwind CSS para el diseño.

Backend: .NET 8 (C#) con arquitectura Web API.

Base de Datos: SQLite (motor relacional ligero y portable).

Documentación de API: Swagger/OpenAPI (para testeo y especificación de endpoints).

Seguridad: Autenticación y Autorización basada en Claims con JWT.

Librerías Clave: Entity Framework Core (ORM), QuestPDF o iTextSharp (Generación de PDF), BCrypt.Net (Hashing de contraseñas).

## 3.0 Modelo Relacional Refinado
Este es el esquema que debes seguir para que no tengas que cambiarlo a mitad de proyecto. He unificado usuarios y añadido los estados:

|Tabla | Campos 
|---|---|
|Usuario | id, dni, nombre, apellido1, apellido2, email, password_hash, rol, activo, fecha_alta |
|Ciclo |	id, nombre, descripcion	Ej: "DAM", "ASIR" |
|Curso |	id, nombre, id_ciclo (FK)	Ej: "1º DAM", "2º DAM". |
|Modulo |	id, nombre, horas, id_curso (FK)	Los módulos pertenecen a un curso. |
| Profesor_Modulo |	id_usuario (FK), id_modulo (FK)	Relación N:M. Quién imparte qué. |
| Matricula |	id, id_usuario (FK), id_curso (FK), fecha, estado	Estado: 'Pendiente', 'Aceptada', 'Rechazada'. |
| Horario |	id, dia_semana, hora_inicio, hora_fin, id_modulo (FK), id_usuario (FK_Profesor)	Cruza qué módulo se da, a qué hora y qué profe lo imparte. |
|Asistencia | id_asistencia (PK), fecha, estado (Pres./Falta/Tarde), id_alumno (FK), id_modulo (FK)	Registro diario de faltas por asignatura. |
| Aviso |	id_aviso (PK), titulo, contenido, fecha_pub, id_autor (FK)	Tablón de anuncios para el Dashboard. |

## Resumen

### 1. Descripción del Proyecto
EduManager es una plataforma integral diseñada para digitalizar la interacción entre el centro educativo, los profesores y los alumnos. El objetivo principal es centralizar la gestión de matrículas, horarios y control de usuarios, permitiendo que cada rol tenga un flujo de trabajo

### 2. Tecnologías Utilizadas (Stack Tecnológico)
Para el desarrollo de la aplicación, he seleccionado una arquitectura moderna y eficiente:

Backend: .NET (ASP.NET Core Web API) para una lógica de negocio.

Base de Datos: SQLite, integrada para facilitar la portabilidad del proyecto. Utilizaré también Entity Framework Core para el mapeo de las clases a tablas en la base de datos.

Frontend: React para hacer toda la interfaz. 

Gestión de Documentos: Creación y posibilidad de descarga de un PDF de la matrícula, luego de ser aceptada

### 3. Funcionalidades Principales
1. La aplicación contará con tres niveles de acceso (Roles):
+ Administrador: Gestión total de usuarios (altas/bajas), creación de ciclos, cursos y módulos, y validación final de matrículas.
+ Profesor: Visualización de sus módulos asignados, alumnos matriculados y consulta de horarios.
+ Alumno: Proceso de matriculación online, consulta de horarios personales y descarga de resúmenes de matrícula en formato PDF.

2. Tendrá un sistema de mensajería para que los profesores envíen mensajes a los alumnos, y los alumnos puedan contestar.

3. Tendrá un sistema de gestión de horarios para registrar la asistencia del alumnado.

### 4. Login y seguridad
El sistema implementará autenticación mediante JWT (JSON Web Tokens) para garantizar que las vistas y los endpoints sean seguros.

### PASOS:

Semana 1: El Motor (Backend y Datos)
Tu objetivo es que al final de esta semana puedas hacer todo desde Swagger. Si Swagger funciona, tienes el 70% del título en el bolsillo.

Día 1-2: Base de Datos y Modelos. Crea todas las clases C#, el DbContext y haz la migración. No pierdas tiempo: si la tabla se crea en SQLite, a por la siguiente.

Día 3: Autenticación (JWT). Es el hueso más duro. Dedícale el día entero a que el Login te devuelva un Token y el Rol del usuario.

Día 4-5: Controladores Críticos. Haz los GET y POST de Usuarios, Matrículas y Módulos. No hagas todos los borrar/editar todavía, céntrate en Crear y Listar.

📅 Semana 2: La Carcasa (React) y PDF
Aquí es donde la app "cobra vida".

Día 6-7: Login y Rutas Protegidas. Haz que React guarde el token y que, dependiendo de si eres Admin o Alumno, te mande a una página u otra.

Día 8-9: Formularios y Tablas. No te compliques con el diseño. Usa una librería de componentes como Tailwind UI o simplemente tablas básicas. Haz que el "Listener" de las asistencias funcione.

Día 10-11: Funcionalidades Especiales. Implementa la generación del PDF y el Tablón de Anuncios.

Día 12-14: Pulido y Errores. Arregla los fallos que salgan (que saldrán) y asegúrate de que el flujo principal (Registrarse -> Matricularse -> Pasar lista) no explote.

💡 3 Reglas de Oro para las 2 semanas:
Usa DTOs desde el minuto 1: No intentes cambiarlo luego, te llevará el doble de tiempo.

No te bloquees: Si un botón no se centra, déjalo feo y sigue con la lógica de la base de datos. Lo que aprueba el FP es la gestión de datos, no el diseño de botones.

Chat e IA con cabeza: Úsalos para generar código repetitivo (como los DTOs o las clases del modelo), pero asegúrate de entender qué hace cada línea, porque en la presentación te van a preguntar.

