---
name: 4geeks-academy
description: Consulta información propia del estudiante en la API de 4Geeks Academy/BreatheCode, incluyendo perfil, cohortes, tareas, certificados, actividad y contenido educativo, sin modificar ni entregar tareas.
---

# 4Geeks Academy

Usa esta skill para responder consultas sobre los datos educativos del propio estudiante mediante la API de BreatheCode. Esta skill describe el flujo, pero **no instala ni conecta por sí misma un cliente API**.

## Límites y disponibilidad

- Utiliza únicamente una herramienta HTTP/API autorizada que esté realmente disponible en el entorno. No inventes nombres de herramientas ni asumas que Mcporter/Zapier proporciona acceso directo a esta API.
- Si no hay una herramienta API disponible, explica que la skill está definida, pero que aún falta conectar una herramienta capaz de consultar la API.
- No uses comandos improvisados que puedan exponer variables de entorno o tokens en argumentos, historial, logs o respuestas.
- Nunca muestres, registres ni solicites al usuario que pegue su token.
- La variable `TOKEN_4GEEKS` está referenciada en la configuración, pero eso por sí solo no demuestra cómo se obtiene ni que contenga un token válido para esta API. Antes de usar endpoints protegidos, debe estar disponible de forma segura y confirmarse que corresponde a un token API.
- Para rutas protegidas, la referencia indica la cabecera `Authorization: Token <token>`. No implementes un login ni infieras cómo renovar credenciales sin documentación y mecanismo confirmados.
- Ante `401` o `403`, detente y explica que la autenticación no funcionó o no tiene autorización. No reintentes con credenciales alternativas.
- La base URL predeterminada es `https://breathecode.herokuapp.com`. Si el usuario o su institución indica otra instancia, confirma la URL antes de consultar.

## Alcance de consultas permitido

Usa únicamente operaciones GET documentadas:

- `GET /v1/admissions/user/me` — perfil del estudiante y academias asociadas.
- `GET /v1/admissions/academy/cohort/me` — cohortes del estudiante; admite filtros como `academy` y `educational_status`.
- `GET /v1/admissions/public/syllabus` — syllabus público; no requiere autenticación.
- `GET /v1/assignment/user/me/task` — tareas propias; admite filtros como `task_status`, `task_type`, `cohort`, `limit` y `offset`.
- `GET /v1/assignment/task/{task_id}` — detalle de una tarea propia seleccionada de la lista del estudiante.
- `GET /v1/certificate/` — certificados propios; admite filtro `cohort`.
- `GET /v1/activity/me` — actividad propia; puede admitir `cohort`, `date_start` y `date_end`.
- `GET /v1/registry/asset` — buscar lecciones, ejercicios y proyectos; puede filtrar por tipo, tecnología, dificultad, texto y límite.
- `GET /v1/registry/asset/{asset_slug}` — detalle de un recurso educativo.
- `GET /v1/registry/asset/me` — recursos creados o modificados por el estudiante.
- `GET /v1/registry/technology` — catálogo de tecnologías.
- `GET /v1/events/all` — eventos; puede filtrar por academia, próximos eventos y límite.

No consultes `GET /v1/activity/cohort/{cohort_id}` en esta skill: puede incluir actividad de otros miembros del cohorte.

## Flujo de trabajo

1. **Clasifica la solicitud.** Identifica qué dato propio quiere consultar el usuario y los filtros necesarios. Esta skill es de solo lectura.
2. **Comprueba la herramienta.** Verifica que haya una herramienta API autorizada y funcional. Si no la hay, no afirmes que consultaste la academia.
3. **Resuelve el contexto cuando haga falta.**
   - Para `academy_id`, consulta el perfil y utiliza `profile_academy` solo cuando la academia sea inequívoca.
   - Para la cohorte activa, consulta las cohortes propias y considera `educational_status: ACTIVE`. Si hay varias cohortes posibles o ninguna coincide, pregunta o explica la ambigüedad.
   - No inventes IDs, slugs ni filtros.
4. **Limita la consulta.** Solicita solo los campos y resultados necesarios. Usa paginación y evita ráfagas de peticiones repetidas.
5. **Protege los datos.** Devuelve únicamente la información pertinente a la pregunta. No expongas tokens, cabeceras de autorización ni datos personales innecesarios.
6. **Verifica y comunica.** Resume los resultados de la respuesta real de la API. Si la llamada falla, indica el error de forma segura y no presentes resultados supuestos como verificados.

## Operaciones no permitidas

No realices llamadas `PUT`, `POST`, `PATCH` ni `DELETE` a esta API. En particular, no actualices tareas ni llames a `/v1/assignment/task/{task_id}/deliver`. Si el usuario pide cambiar o entregar una tarea, explica que esta versión solo permite consultas y no ejecutes la acción.
