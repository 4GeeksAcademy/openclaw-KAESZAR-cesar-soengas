 ---                                                                                                                                
  name: 4geeks-projects                                                                                                              
  description: Consulta los proyectos del estudiante en 4Geeks Academy/BreatheCode con filtros — todos, pendientes, entregados,      
calificados — sin código por parte del usuario.                                                                                      
  ---                                                                                                                                
                                                                                                                                     
  # 4Geeks Projects                                                                                                                  
                                                                                                                                     
  Skill de **responsabilidad única**: consultar proyectos del estudiante. No se mezcla con autenticación, perfil, progreso ni otras  
consultas.                                                                                                                           
                                                                                                                                     
  ## Seguridad                                                                                                                       
                                                                                                                                     
  - **Nunca muestres, registres ni expongas** el valor de `TOKEN_4GEEKS`.                                                            
  - Usa siempre `Authorization: Token ***}` para expansión segura.                                                                   
  - Ante `401` o `403`, informa que el token expiró y pide uno nuevo en https://api-token.breathecode.io.                            
  - Solo `GET`; prohibidos `POST`, `PUT`, `PATCH`, `DELETE`.                                                                         
                                                                                                                                     
  ## Base URL                                                                                                                        
                                                                                                                                     
  `https://breathecode.herokuapp.com`                                                                                                
                                                                                                                                     
  ## Endpoints                                                                                                                       
                                                                                                                                     
```                                                                                                                                  
                                                                                                                                     
GET /v1/assignment/user/me/task?task_type=PROJECT&limit=50                                                                           
                                                                                                                                     
```                                                                                                                                  
                                                                                                                                     
  ### Filtros disponibles                                                                                                            
                                                                                                                                     
  | Filtro          | Parámetro                           |                                                                          
  |-----------------|-------------------------------------|                                                                          
  | Pendientes      | `task_status=PENDING`              |                                                                           
  | Entregados      | `task_status=DONE`                 |                                                                           
  | Calificados     | `revision_status=APPROVED`         |                                                                           
  | Pte. revisión   | `revision_status=PENDING`          |                                                                           
  | Por cohorte     | `cohort={cohort_id}`               |                                                                           
  | Combinados      | Ej: `task_status=DONE&revision_status=APPROVED` |                                                              
                                                                                                                                     
  ### Campos relevantes                                                                                                              
                                                                                                                                     
  `id`, `title`, `task_status`, `revision_status`, `github_url`, `live_url`, `delivered_at`, `reviewed_at`, `cohort.name`            
                                                                                                                                     
  ## Procesamiento recomendado                                                                                                       
                                                                                                                                     
  ```bash                                                                                                                            
  curl -s -H "Authorization: Token ***}" "<endpoint>" | python3 -c "                                                                 
  import json,sys                                                                                                                    
  d=json.load(sys.stdin)                                                                                                             
  items=d.get('results',d) if isinstance(d,dict) else d                                                                              
  print(f'Total: {d.get(\"count\", len(items))}')                                                                                    
  for i in items:                                                                                                                    
      status = i['task_status']                                                                                                      
      rev = i['revision_status']                                                                                                     
      cohort = i['cohort']['name']                                                                                                   
      print(f\"{i['title']}\")                                                                                                       
      print(f'  Estado: {\"PENDIENTE\" if status==\"PENDING\" else \"ENTREGADO\"} | Revisión: {\"PENDIENTE\" if rev==\"PENDING\" else
\"CALIFICADO\"}')                                                                                                                    
      print(f'  Cohort: {cohort}')                                                                                                   
      if i.get('github_url'): print(f'  GitHub: {i[\"github_url\"]}')                                                                
      if i.get('delivered_at'): print(f'  Entregado: {i[\"delivered_at\"][:10]}')                                                    
      if i.get('reviewed_at'): print(f'  Calificado: {i[\"reviewed_at\"][:10]}')                                                     
      print()                                                                                                                        
  "            