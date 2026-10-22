  ---                                                                                                                                
  name: 4geeks-progress                                                                                                              
  description: Proporciona un resumen de avance del estudiante en 4Geeks Academy — tareas completadas, pendientes, calificadas y     
progreso porcentual por cohorte.                                                                                                     
  ---                                                                                                                                
                                                                                                                                     
  # 4Geeks Progress                                                                                                                  
                                                                                                                                     
  Skill de **responsabilidad única**: calcular y presentar el progreso/avance del estudiante en sus cursos. No se mezcla con         
autenticación, proyectos individuales ni consultas de perfil.                                                                        
                                                                                                                                     
  Usa `exec` con `curl` y la variable de entorno `TOKEN_4GEEKS` — ya configurada en `.env` y `openclaw.json`.                        
                                                                                                                                     
  ## Requisitos verificados                                                                                                          
                                                                                                                                     
  - `TOKEN_4GEEKS` está definida en `.env` y referenciada en `openclaw.json` como `${TOKEN_4GEEKS}`.                                 
  - El token fue probado contra `GET /v1/admissions/user/me` y responde correctamente.                                               
  - `curl` disponible.                                                                                                               
                                                                                                                                     
  ## Seguridad                                                                                                                       
                                                                                                                                     
  - **Nunca muestres, registres ni expongas** el valor de `TOKEN_4GEEKS` en la conversación, logs, respuestas ni argumentos de       
comandos.                                                                                                                            
  - Usa siempre `Authorization: Token ${TOKEN_4GEEKS}` dentro del comando shell.                                                     
  - Ante `401` o `403`, informa al usuario que el token expiró y pídele uno nuevo en https://api-token.breathecode.io.               
  - Solo `GET`; prohibidos `POST`, `PUT`, `PATCH`, `DELETE`.                                                                         
                                                                                                                                     
  ## Endpoint base                                                                                                                   
                                                                                                                                     
```                                                                                                                                  
                                                                                                                                     
curl -s -H "Authorization: Token ${TOKEN_4GEEKS}" "https://breathecode.herokuapp.com/v1/assignment/user/me/task?limit=100"           
                                                                                                                                     
```                                                                                                                                  
                                                                                                                                     
  El límite de 100 es suficiente para la mayoría de estudiantes; si hay más tareas, paginar con `&page=2`.                           
                                                                                                                                     
  ## Procesamiento para resumen de progreso                                                                                          
                                                                                                                                     
  Usa siempre Python para procesar el JSON porque es más fiable que `jq` para agrupaciones complejas.                                
                                                                                                                                     
  ### Procesamiento completo por cohorte                                                                                             
                                                                                                                                     
  ```bash                                                                                                                            
  curl -s -H "Authorization: Token ${TOKEN_4GEEKS}" "https://breathecode.herokuapp.com/v1/assignment/user/me/task?limit=100" |       
python3 -c "                                                                                                                         
  import json, sys                                                                                                                   
  d = json.load(sys.stdin)                                                                                                           
  items = d.get('results', d) if isinstance(d, dict) else d                                                                          
                                                                                                                                     
  from collections import defaultdict                                                                                                
  by_cohort = defaultdict(lambda: {                                                                                                  
      'total': 0, 'done': 0, 'pending': 0,                                                                                           
      'approved': 0, 'pending_review': 0,                                                                                            
      'exercises': 0, 'projects': 0, 'lessons': 0                                                                                    
  })                                                                                                                                 
                                                                                                                                     
  for i in items:                                                                                                                    
      cn = i['cohort']['name']                                                                                                       
      by_cohort[cn]['total'] += 1                                                                                                    
      if i['task_status'] == 'DONE':                                                                                                 
          by_cohort[cn]['done'] += 1                                                                                                 
      else:                                                                                                                          
          by_cohort[cn]['pending'] += 1                                                                                              
      if i['revision_status'] == 'APPROVED':                                                                                         
          by_cohort[cn]['approved'] += 1                                                                                             
      else:                                                                                                                          
          by_cohort[cn]['pending_review'] += 1                                                                                       
      tt = i.get('task_type', 'OTHER')                                                                                               
      if tt == 'EXERCISE': by_cohort[cn]['exercises'] += 1                                                                           
      elif tt == 'PROJECT': by_cohort[cn]['projects'] += 1                                                                           
      elif tt == 'LESSON': by_cohort[cn]['lessons'] += 1                                                                             
                                                                                                                                     
  print(f'📊 Resumen de Progreso — {len(items)} tareas totales')                                                                     
  print()                                                                                                                            
  for cn, s in sorted(by_cohort.items(), key=lambda x: -x[1]['total']):                                                              
      pct = round(s['done'] / s['total'] * 100) if s['total'] else 0                                                                 
      print(f'📁 {cn}')                                                                                                              
      print(f'   Progreso: {s[\"done\"]}/{s[\"total\"]} ({pct}%)')                                                                   
      print(f'   Pendientes: {s[\"pending\"]} | Calificados: {s[\"approved\"]} | Pend.revisión: {s[\"pending_review\"]}')            
      print(f'   Ejercicios: {s[\"exercises\"]} | Proyectos: {s[\"projects\"]} | Lecciones: {s[\"lessons\"]}')                       
      print()                                                                                                                        
  "                                                                                                                                  
```                                                                                                                                  
                                                                                                                                     
Flujo de trabajo                                                                                                                     
                                                                                                                                     
1. Detecta la intención. El usuario pide "mi progreso", "avance en el curso", "cómo voy", "qué me falta", etc.                       
2. Ejecuta el curl al endpoint de tareas con limit=100.                                                                              
3. Procesa con Python el JSON agrupando por cohorte.                                                                                 
4. Devuelve un resumen amigable:                                                                                                     
    - Por cada cohorte: porcentaje de avance, tareas hechas/pendientes/calificadas                                                   
    - Desglose por tipo (ejercicios, proyectos, lecciones)                                                                           
    - Si una cohorte está al 100%, destacarlo como completada                                                                        
    - Si hay cohortes con 0% de avance, señalarlo como "sin comenzar"                                                                
5. Si el usuario quiere detalle de una cohorte en concreto, puedes mostrar también el listado de tareas pendientes de esa cohorte.   
                                                                                                                                     
Operaciones NO permitidas                                                                                                            
                                                                                                                                     
- No hagas llamadas POST, PUT, PATCH ni DELETE.                                                                                      
- No entregues tareas.                                                                                                               
- No calcules progreso de cohortes ajenas.                                                                                           
                                                                                                                                     
Resolución de errores                                                                                                                
                                                                                                                                     
- 401/403: Token expirado. Pedir nuevo token en https://api-token.breathecode.io.                                                    
- Red vacía: Informar del código HTTP sin inventar datos.                                                                            
- Más de 100 tareas: El endpoint devuelve count. Si count > 100, usar paginación con &page=N y sumar los resultados.                 
                                                                                                                                     
Ejemplos de consultas del usuario                                                                                                    
                                                                                                                                     
┌────────────────────────────────────────────┬─────────────────────────────────────────────────┐                                     
│ Pregunta del usuario                       │ Acción                                          │                                     
├────────────────────────────────────────────┼─────────────────────────────────────────────────┤                                     
│ "Cómo voy en el curso" / "Mi progreso"     │ Resumen completo por cohorte                    │                                     
├────────────────────────────────────────────┼─────────────────────────────────────────────────┤                                     
│ "Qué me falta por hacer en AI Engineering" │ Filtro por cohorte específico + pendientes      │                                     
├────────────────────────────────────────────┼─────────────────────────────────────────────────┤                                     
│ "Cuántos proyectos me faltan por entregar" │ Resumen solo de proyectos, agrupado por cohorte │                                     
├────────────────────────────────────────────┼─────────────────────────────────────────────────┤                                     
│ "Qué cohortes tengo al día"                │ Mostrar solo cohortes con 100%                  │                                     
├────────────────────────────────────────────┼─────────────────────────────────────────────────┤                                     
│ "Dónde voy más atrasado"                   │ Ordenar por % ascendente, destacar la peor      │                                     
└────────────────────────────────────────────┴─────────────────────────────────────────────────┘                                     
                                                                                                                                     
                           