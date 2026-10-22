 ---                                                                                                                                
  name: 4geeks-connect                                                                                                               
  description: Consulta en tiempo real los datos del estudiante en 4Geeks Academy/BreatheCode usando el token API ya configurado —   
perfil, tareas, cohortes, certificados, actividad y recursos educativos. Sin necesidad de que el usuario escriba código.             
  ---                                                                                                                                
                                                                                                                                     
  # 4Geeks Connect                                                                                                                   
                                                                                                                                     
  Skill operativa para consultar la API de BreatheCode de 4Geeks Academy. Usa `exec` con `curl` y la variable de entorno             
`TOKEN_4GEEKS` — que ya está configurada — para hacer peticiones autenticadas.                                                       
                                                                                                                                     
  ## Requisitos verificados                                                                                                          
                                                                                                                                     
  - `TOKEN_4GEEKS` está definida en el archivo `.env` del workspace y referenciada en `openclaw.json` como `${TOKEN_4GEEKS}`.        
  - El token ya fue probado contra `GET /v1/admissions/user/me` y responde correctamente.                                            
  - `curl` está disponible en el sistema.                                                                                            
                                                                                                                                     
  ## Seguridad                                                                                                                       
                                                                                                                                     
  - **Nunca muestres, registres ni expongas** el valor de `TOKEN_4GEEKS` en la conversación, logs, respuestas ni argumentos de       
comandos.                                                                                                                            
  - Para las llamadas, usa siempre la sintaxis `Authorization: Token $TOKEN_4GEEKS` dentro del comando shell.                        
  - Ante un `401` o `403`, informa al usuario que el token no funciona o expiró, y pídele que genere uno nuevo en                    
https://api-token.breathecode.io.                                                                                                    
  - No uses herramientas HTTP genéricas no disponibles; usa `exec` con `curl`.                                                       
                                                                                                                                     
  ## Base URL                                                                                                                        
                                                                                                                                     
  `https://breathecode.herokuapp.com`                                                                                                
                                                                                                                                     
  ## Llamadas disponibles                                                                                                            
                                                                                                                                     
  ### Perfil del estudiante                                                                                                          
  ```bash                                                                                                                            
  curl -s -H "Authorization: Token $TOKEN_4GEEKS" "https://breathecode.herokuapp.com/v1/admissions/user/me"                          
```                                                                                                                                  
                                                                                                                                     
### Cohortes del estudiante                                                                                                          
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS" "https://breathecode.herokuapp.com/v1/admissions/academy/cohort/me"                
```                                                                                                                                  
                                                                                                                                     
Filtro por estado educativo activo:                                                                                                  
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS"                                                                                    
"https://breathecode.herokuapp.com/v1/admissions/academy/cohort/me?educational_status=ACTIVE"                                        
```                                                                                                                                  
                                                                                                                                     
### Tareas propias                                                                                                                   
                                                                                                                                     
Todas:                                                                                                                               
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS" "https://breathecode.herokuapp.com/v1/assignment/user/me/task"                     
```                                                                                                                                  
                                                                                                                                     
Con filtros:                                                                                                                         
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS"                                                                                    
"https://breathecode.herokuapp.com/v1/assignment/user/me/task?task_status=PENDING&limit=50"                                          
```                                                                                                                                  
                                                                                                                                     
### Detalle de una tarea                                                                                                             
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS" "https://breathecode.herokuapp.com/v1/assignment/task/{task_id}"                   
```                                                                                                                                  
                                                                                                                                     
### Certificados                                                                                                                     
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS" "https://breathecode.herokuapp.com/v1/certificate/"                                
```                                                                                                                                  
                                                                                                                                     
### Actividad reciente                                                                                                               
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS" "https://breathecode.herokuapp.com/v1/activity/me?limit=20"                        
```                                                                                                                                  
                                                                                                                                     
### Buscar recursos educativos                                                                                                       
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS" "https://breathecode.herokuapp.com/v1/registry/asset?limit=10"                     
```                                                                                                                                  
                                                                                                                                     
Por tipo (LESSON, EXERCISE, PROJECT, QUIZ):                                                                                          
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS" "https://breathecode.herokuapp.com/v1/registry/asset?type=EXERCISE&limit=10"       
```                                                                                                                                  
                                                                                                                                     
### Detalle de un recurso por slug                                                                                                   
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS" "https://breathecode.herokuapp.com/v1/registry/asset/{asset_slug}"                 
```                                                                                                                                  
                                                                                                                                     
### Tecnologías disponibles                                                                                                          
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS" "https://breathecode.herokuapp.com/v1/registry/technology"                         
```                                                                                                                                  
                                                                                                                                     
### Eventos                                                                                                                          
                                                                                                                                     
```bash                                                                                                                              
  curl -s -H "Authorization: Token $TOKEN_4GEEKS" "https://breathecode.herokuapp.com/v1/events/all"                                  
```                                                                                                                                  
                                                                                                                                     
Flujo de trabajo                                                                                                                     
                                                                                                                                     
1. Clasifica la consulta.                                                                                                            
2. Ejecuta el curl correspondiente.                                                                                                  
3. Procesa el JSON con jq o python3 -m json.tool.                                                                                    
4. Devuelve un resumen legible (nada de JSON crudo salvo que el usuario lo pida).                                                    
5. Manejo de errores: muestra el código HTTP, no inventes datos.                                                                     
                                                                                                                                     
Operaciones NO permitidas                                                                                                            
                                                                                                                                     
- No hagas llamadas POST, PUT, PATCH ni DELETE.                                                                                      
- No entregues tareas.                                                                                                               
- No consultes actividad de cohortes ajenas.                                                                                         
                                                                                                                                     
Resolución de contexto                                                                                                               
                                                                                                                                     
- Para identificar la academia activa, consulta el perfil primero.                                                                   
- Si hay varias cohortes activas, confirma con el usuario cuál usar.                                                                 
- Si no hay jq, usa python3 -c "import sys,json; d=json.load(sys.stdin); ..." para procesar.                                         
```                                                            