 ---                                                                                                                                
  name: 4geeks-activity                                                                                                              
  description: Consulta eventos, recursos educativos y activity stream del estudiante en 4Geeks Academy/BreatheCode usando endpoints 
alternativos que la cuenta sí puede consumir.                                                                                        
  ---                                                                                                                                
                                                                                                                                     
  # 4Geeks Activity                                                                                                                  
                                                                                                                                     
  Skill de **responsabilidad única**: consultar actividad del estudiante — eventos próximos/pasados (`/v1/events`), recursos         
educativos (`/v1/registry/asset`) y cualquier endpoint de solo lectura accesible.                                                    
  **No** incluye autenticación, perfil, tareas, proyectos, certificados ni progreso.                                                 
                                                                                                                                     
  ## Límites                                                                                                                         
                                                                                                                                     
  - Solo lectura (`GET`). **Prohibido** enviar `POST`, `PUT`, `PATCH` o `DELETE`.                                                    
  - No mezclar con otras responsabilidades.                                                                                          
  - `/v1/activity/me` requiere `read_activity` y header `Academy: <id>`. Esta cuenta **no** tiene ese permiso. Usar alternativas     
documentadas.                                                                                                                        
  - Las alternativas (`/v1/events/me`, `/v1/events/all`, `/v1/registry/asset`) **no** requieren header `Academy`.                    
                                                                                                                                     
  ## Uso                                                                                                                             
                                                                                                                                     
  Usa `python3` para procesar JSON. Activa venv si existe:                                                                           
                                                                                                                                     
  ```sh                                                                                                                              
  source /root/.openclaw/workspace/.venv/bin/activate                                                                                
```                                                                                                                                  
                                                                                                                                     
Cargar token:                                                                                                                        
                                                                                                                                     
```sh                                                                                                                                
  TOKEN_4GEEKS=$(grep ^TOKEN_4GEEKS /root/.openclaw/workspace/.env | cut -d= -f2)                                                    
```                                                                                                                                  
                                                                                                                                     
Base URL: https://breathecode.herokuapp.com                                                                                          
                                                                                                                                     
Endpoints disponibles                                                                                                                
                                                                                                                                     
### 1. Eventos del estudiante — /v1/events/me                                                                                        
                                                                                                                                     
Devuelve eventos a los que el estudiante está inscrito.                                                                              
                                                                                                                                     
```sh                                                                                                                                
  curl -s -H "Authorization: Token ${TOKEN_4GEEKS}" \                                                                                
    "https://breathecode.herokuapp.com/v1/events/me" \                                                                               
    | python3 -m json.tool                                                                                                           
```                                                                                                                                  
                                                                                                                                     
### 2. Todos los eventos — /v1/events/all                                                                                            
                                                                                                                                     
Devuelve todos los eventos públicos de la plataforma.                                                                                
                                                                                                                                     
```sh                                                                                                                                
  curl -s -H "Authorization: Token ${TOKEN_4GEEKS}" \                                                                                
    "https://breathecode.herokuapp.com/v1/events/all" \                                                                              
    | python3 -m json.tool                                                                                                           
```                                                                                                                                  
                                                                                                                                     
### 3. Assets del registry — /v1/registry/asset                                                                                      
                                                                                                                                     
Devuelve recursos educativos (ejercicios, lecciones). Soporta paginacin con ?limit= y ?offset=.                                      
                                                                                                                                     
```sh                                                                                                                                
  curl -s -H "Authorization: Token ${TOKEN_4GEEKS}" \                                                                                
    "https://breathecode.herokuapp.com/v1/registry/asset?limit=10"                                                                  
```                                                                                                                                  
                                                                                                                                     
Respuestas ante errores                                                                                                              
                                                                                                                                     
- 401/403 sin mensaje de permiso: Token inválido/inactivo. Pedir nuevo token.                                                        
- 403 con don't have this capability: read_activity: Cuenta no tiene permiso. Usar alternativa documentada.                          
- Error de conexión o DNS: API temporalmente no disponible.                                                                          
```