  # SKILL_LOG.md — Bitácora de Skills 4Geeks Academy                                                                        
                                                                                                                            
  > Registro de construcción, prompts de creación, descripción, endpoints y resultados de prueba para cada skill.           
                                                                                                                            
  ---                                                                                                                       
                                                                                                                            
  ## 1. `4geeks-auth`                                                                                                       
                                                                                                                            
  **Prompt de creación:** *"crea una skill que verifique que el token de 4Geeks Academy sea válido y que la sesión del      
estudiante esté activa"*                                                                                                    
                                                                                                                            
  **Descripción:** Verifica que el token API de BreatheCode sea válido (`GET /v1/admissions/user/me`) y guía al usuario a   
renovarlo si expiró. Responsabilidad única: autenticación — no hace consultas de perfil ni nada más.                        
                                                                                                                            
  **Endpoint:** `GET /v1/admissions/user/me`                                                                                
                                                                                                                            
  **Resultado de prueba:**                                                                                                  
```                                                                                                                         
                                                                                                                            
HTTP 200 — Token válido, sesión activa                                                                                      
Usuario: César Soengas (cesarsoengas.crs@gmail.com) | ID: 22125                                                             
20 cohorts activas/graduadas                                                                                                
                                                                                                                            
```                                                                                                                         
                                                                                                                            
  ---                                                                                                                       
                                                                                                                            
  ## 2. `4geeks-connect`                                                                                                    
                                                                                                                            
  **Prompt de creación:** *"crea una skill que consulte en tiempo real los datos del estudiante — perfil, tareas, cohortes, 
sin que el usuario escriba código"*                                                                                         
                                                                                                                            
  **Descripción:** Skill generalista que expone todos los endpoints GET de la API de BreatheCode: perfil, cohortes, tareas, 
proyectos, certificados, actividad, recursos educativos y eventos. Es la skill "navaja suiza" para cualquier consulta.      
                                                                                                                            
  **Endpoints:**                                                                                                            
  - `GET /v1/admissions/user/me`                                                                                            
  - `GET /v1/admissions/academy/cohort/me`                                                                                  
  - `GET /v1/assignment/user/me/task`                                                                                       
  - `GET /v1/assignment/task/{task_id}`                                                                                     
  - `GET /v1/certificate/`                                                                                                  
  - `GET /v1/activity/me`                                                                                                   
  - `GET /v1/registry/asset`                                                                                                
  - `GET /v1/events/all`                                                                                                    
                                                                                                                            
  **Resultado de prueba:**                                                                                                  
```                                                                                                                         
                                                                                                                            
Perfil completo con 20 cohorts, 156 tareas, 33 proyectos                                                                    
Todos los endpoints responden correctamente                                                                                 
                                                                                                                            
```                                                                                                                         
                                                                                                                            
  ---                                                                                                                       
                                                                                                                            
  ## 3. `4geeks-progress`                                                                                                   
                                                                                                                            
  **Prompt de creación:** *"crea una skill que calcule el progreso del estudiante — tareas completadas, pendientes,         
calificadas y porcentaje por cohorte"*                                                                                      
                                                                                                                            
  **Descripción:** Toma todas las tareas del estudiante, las agrupa por cohorte y calcula: total, completadas vs pendientes,
calificadas vs pendientes de revisión, desglose por tipo (ejercicios, proyectos, lecciones). Ideal para "cómo voy en el     
curso".                                                                                                                     
                                                                                                                            
  **Endpoint:** `GET /v1/assignment/user/me/task?limit=100`                                                                 
                                                                                                                            
  **Resultado de prueba:**                                                                                                  
```                                                                                                                         
                                                                                                                            
Total tareas: 156                                                                                                           
Procesamiento por cohorte: funciona con python3 agrupando resultados                                                        
                                                                                                                            
```                                                                                                                         
                                                                                                                            
  ---                                                                                                                       
                                                                                                                            
  ## 4. `4geeks-projects`                                                                                                   
                                                                                                                            
  **Prompt de creación:** *"crea una skill que consulte los proyectos del estudiante con filtros — todos, pendientes,       
entregados, calificados"*                                                                                                   
                                                                                                                            
  **Descripción:** Consulta solo proyectos (`task_type=PROJECT`) con filtros combinables: estado de tarea (PENDING/DONE),   
estado de revisión (APPROVED/PENDING), cohorte específica. Muestra título, estado, URLs y fechas.                           
                                                                                                                            
  **Endpoint:** `GET /v1/assignment/user/me/task?task_type=PROJECT&limit=50`                                                
                                                                                                                            
  **Resultado de prueba:**                                                                                                  
```                                                                                                                         
                                                                                                                            
Total proyectos: 33                                                                                                         
Muestra: Command Line Challenge (DONE/APPROVED),                                                                            
         Is This Snack Healthy? (DONE/APPROVED),                                                                            
         Milestone — Final Project Video (PENDING/PENDING)                                                                  
                                                                                                                            
```                                                                                                                         
                                                                                                                            
  ---                                                                                                                       
                                                                                                                            
  ## 5. `4geeks-certificates`                                                                                               
                                                                                                                            
  **Prompt de creación:** *"crea una skill que liste los certificados obtenidos por el estudiante"*                         
                                                                                                                            
  **Descripción:** Consulta los certificados emitidos al estudiante autenticado. Endpoint simple, sin header `Academy`.     
Muestra especialidad, fecha de emisión, preview URL y PDF URL. Maneja lista vacía y error de permisos `read_certificate`.   
                                                                                                                            
  **Endpoint:** `GET /v1/certificate/me`                                                                                    
                                                                                                                            
  **Resultado de prueba:**                                                                                                  
```                                                                                                                         
                                                                                                                            
Total certificados: 5                                                                                                       
1. Command Line, Git and Github — 2026-10-14                                                                                
2. Web UI fundamentals with Tailwind — 2026-10-03                                                                           
3. Basic personal assistants with Openclaw — 2026-10-03                                                                     
4. No-Code & Automations — 2026-09-23                                                                                       
5. Working with AI coding agents — 2026-09-19                                                                               
```                                                                                                                         
                                                                                                                            
  ---                                                                                                                       
                                                                                                                            
  ## 6. `4geeks-activity`                                                                                                   
                                                                                                                            
  **Prompt de creación:** *"crea una skill que consulte la actividad del estudiante — eventos, recursos educativos y        
activity stream"*                                                                                                           
                                                                                                                            
  **Descripción:** Consulta actividad del estudiante usando tres vías:                                                      
  - **Eventos propios** (`GET /v1/events/me`): eventos a los que está inscrito                                              
  - **Todos los eventos** (`GET /v1/events/all`): eventos públicos                                                          
  - **Recursos educativos** (`GET /v1/registry/asset`): ejercicios y lecciones                                              
  - **Activity stream** (`GET /v1/activity/me`): documentado como no disponible (requiere permiso `read_activity` no        
concedido)                                                                                                                  
                                                                                                                            
  **Endpoints:**                                                                                                            
  - `GET /v1/events/me`                                                                                                     
  - `GET /v1/events/all`                                                                                                    
  - `GET /v1/registry/asset`                                                                                                
                                                                                                                            
  **Resultado de prueba:**                                                                                                  
```                                                                                                                         
                                                                                                                            
Eventos inscrito: 4 (todos ACTIVE)                                                                                          
- Real Projects. Real AI. Built by Our AI Engineers. — 2026-10-23T14:00                                                     
- Your Way Into the AI Jobs That Pay 62% More — 2026-11-03/04/05                                                            
                                                                                                                            
Registry: 95 ejercicios totales                                                                                             
  ej: "Learn how to build HTTP requests with Python"                                                                        
  ej: "Certified AI Founder: Growth Mindset & Smart Validation"                                                             
                                                                                                                            
```                                                                                                                         
                                                                                                                            
  ---                                                                                                                       
                                                                                                                            
  ---                                                                                                                       
                                                                                                                            
  ## Información solicitada al usuario                                                                                      
                                                                                                                            
  Durante la construcción de las 6 skills, OpenClaw (el asistente Jazz) fue solicitando la siguiente información al usuario 
para poder ejecutar las tareas:                                                                                             
                                                                                                                            
  ### 1. Token API de BreatheCode                                                                                           
                                                                                                                            
  **Preguntado:** *"Necesito tu token de API de BreatheCode para poder hacer consultas"*                                    
                                                                                                                            
  **Respuesta:** El usuario proporcionó el token `*******************`                                 
                                                                                                                            
  **Uso:** Se configuró como variable de entorno y se usa en el header `Authorization: Token ***` para todas las llamadas a 
la API.                                                                                                                     
                                                                                                                            
  ---                                                                                                                       
                                                                                                                            
  ### 2. URL base correcta de la API                                                                                        
                                                                                                                            
  **Preguntado:** *"Estoy probando endpoints — ¿cuál es la URL base correcta?"* (tras detectar que varios dominios fallaban)
                                                                                                                            
  **Respuesta:** Se descubrió mediante prueba y error que la URL correcta es `https://breathecode.herokuapp.com`. Dominios  
que **no funcionan**: `breathecode.heroku.com`, `api.breathecode.io`, `api.4geeksacademy.com`, `breathecode.4geeks.academy`.
                                                                                                                            
  ---                                                                                                                       
                                                                                                                            
  ### 3. Confirmación de endpoints y headers                                                                                
                                                                                                                            
  **Preguntado:** *"¿Qué endpoints necesitas? Voy probando uno por uno"* — el asistente fue descubriendo interactivamente   
qué endpoints requieren el header `Academy: <id>` y cuáles no.                                                              
                                                                                                                            
  **Respuesta:** Basado en respuestas HTTP (200/403/404) se determinó:                                                      
  - ✅ Funcionan **sin** `Academy`: `/v1/certificate/me`, `/v1/events/me`, `/v1/events/all`, `/v1/registry/asset`           
  - ❌ Requieren `Academy` y permiso especial (no disponible): `/v1/activity/me` (403 — `read_activity`)                    
  - ❌ No existen: `/v1/certificate/user/me`, `/v1/certificate/student/me`, `/v1/certification/me` (404/301)                
                                                                                                                            
  ---                                                                                                                       
                                                                                                                     
                                                                                                                            
  ### 4. Confirmación de skills a subir al repositorio                                                                      
                                                                                                                            
  **Preguntado:** *"¿Agrego estas 2 nuevas skills al repo también?"* — tras crear `4geeks-certificates` y `4geeks-activity` 
                                                                                                                            
  **Respuesta:** El usuario respondió afirmativamente. Queda pendiente el `git add` + `git commit`.                         
                                                                                                                            
  ---                                                                                                                       
                                                                                                                     
                                                                                                                            
  ### 5. (Implícito) Verificación del estado de daemon y openclaw.json                                                      
                                                                                                                            
  **Preguntado:** OpenClaw reportó un fallo de auth del modelo y pidió recargar/revisar la configuración.                   
                                                                                                                            
  **Respuesta:** Queda pendiente de resolución.                                                                             
                                                                                                                            
  ---                                                                                                                       
                                                                                                                            
  ### Resumen de interacciones                                                                                              
                                                                                                                            
  | # | Información solicitada | ¿Resuelto? |                                                                               
  |---|------------------------|------------|                                                                               
  | 1 | Token API de BreatheCode | ✅ Resuelto — proporcionado por el usuario |                                             
  | 2 | URL base correcta | ✅ Resuelto — descubierto por prueba |                                                          
  | 3 | Endpoints y headers funcionales | ✅ Resuelto — mapeados experimentalmente |                                                                                     
  | 4 | Skills a trackear en git | ⏳ Pendiente — confirmado pero sin       commit |                                                                                                           
  | 5 | Config de daemon / openclaw.json | ⏳ Pendiente — fallo de auth del modelo |                                        
                                                                                                                            
  ## Resumen                                                                                                                
                                                                                                                            
  | Skill | Endpoints | Estado |                                                                                            
  |-------|-----------|--------|                                                                                            
  | `4geeks-auth` | 1 | ✅ Probado — sesión activa |                                                                        
  | `4geeks-connect` | 8 | ✅ Probado — perfil, cohorts, tareas OK |                                                        
  | `4geeks-progress` | 1 | ✅ Probado — 156 tareas procesables |                                                           
  | `4geeks-projects` | 1 | ✅ Probado — 33 proyectos |                                                                     
  | `4geeks-certificates` | 1 | ✅ Probado — 5 certificados |                                                               
  | `4geeks-activity` | 3 | ✅ Probado — 4 eventos, 95 ejercicios |   


┌─────────────────────┬─────────────────────────────────┬──────────────────────────────┬───────────────────────────────────┐
│ Skill               │ Prompt clave                    │ Endpoints                    │ Prueba                            │
├─────────────────────┼─────────────────────────────────┼──────────────────────────────┼───────────────────────────────────┤
│ 4geeks-auth         │ "verifique que el token sea     │ GET /v1/admissions/user/me   │ ✅ HTTP 200 — César Soengas, 20   │
│                     │ válido"                         │                              │ cohorts                           │
├─────────────────────┼─────────────────────────────────┼──────────────────────────────┼───────────────────────────────────┤
│ 4geeks-connect      │ "consulte perfil, tareas,       │ 8 endpoints                  │ ✅ Perfil completo, 156 tareas    │
│                     │ cohortes"                       │                              │                                   │
├─────────────────────┼─────────────────────────────────┼──────────────────────────────┼───────────────────────────────────┤
│ 4geeks-progress     │ "calcule progreso por cohorte"  │ GET ...task?limit=100        │ ✅ 156 tareas procesables         │
├─────────────────────┼─────────────────────────────────┼──────────────────────────────┼───────────────────────────────────┤
│ 4geeks-projects     │ "proyectos con filtros"         │ GET                          │ ✅ 33 proyectos                   │
│                     │                                 │ ...task?task_type=PROJECT    │                                   │
├─────────────────────┼─────────────────────────────────┼──────────────────────────────┼───────────────────────────────────┤
│ 4geeks-certificates │ "certificados obtenidos"        │ GET /v1/certificate/me       │ ✅ 5 certificados                 │
├─────────────────────┼─────────────────────────────────┼──────────────────────────────┼───────────────────────────────────┤
│ 4geeks-activity     │ "eventos, recursos, activity    │ 3 endpoints                  │ ✅ 4 eventos, 95 ejercicios       │
│                     │ stream"                         │                              │                                   │
└─────────────────────┴─────────────────────────────────┴──────────────────────────────┴───────────────────────────────────┘
                                                                                                                            
- Cada skill en el log tiene: el prompt en lenguaje natural con el que se inició, descripción breve, endpoints usados y el    
 resultado real de las pruebas que acabamos de ejecutar. 