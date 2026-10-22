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