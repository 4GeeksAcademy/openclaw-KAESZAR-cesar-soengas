 ---                                                                                                                                
  name: 4geeks-auth                                                                                                                  
  description: Verifica que el token de 4Geeks Academy/BreatheCode sea válido y que la sesión del estudiante esté activa antes de que
otras skills operen. Responsabilidad única: autenticación.                                                                           
  ---                                                                                                                                
                                                                                                                                     
  # 4Geeks Auth                                                                                                                      
                                                                                                                                     
  Skill con **responsabilidad única**: verificar que el token API de 4Geeks Academy sea válido, que la sesión esté activa y, si no,  
guiar al usuario para renovarlo.                                                                                                     
                                                                                                                                     
  No hace consultas de perfil, tareas, cohortes ni nada más. Esa es responsabilidad de otras skills.                                 
                                                                                                                                     
  ## Requisitos                                                                                                                      
                                                                                                                                     
  - `TOKEN_4GEEKS` definida en `.env` del workspace                                                                                  
  - `curl` disponible                                                                                                                
                                                                                                                                     
  ## Seguridad                                                                                                                       
                                                                                                                                     
  - **Nunca muestres, registres ni expongas** el valor de `TOKEN_4GEEKS`.                                                            
  - No guardes el token en logs, respuestas ni variables de sesión.                                                                  
  - Si el token falla, indica al usuario exactamente qué hacer.                                                                      
                                                                                                                                     
  ## Endpoint                                                                                                                        
                                                                                                                                     
```                                                                                                                                  
                                                                                                                                     
GET https://breathecode.herokuapp.com/v1/admissions/user/me                                                                          
                                                                                                                                     
```                                                                                                                                  
                                                                                                                                     
  ## Funciones                                                                                                                       
                                                                                                                                     
  ### `verify_token()` — Verificar que el token es válido                                                                            
                                                                                                                                     
  ```sh                                                                                                                              
  curl -s -w "\n%{http_code}" -H "Authorization: Token ***}" \                                                                       
    "https://breathecode.herokuapp.com/v1/admissions/user/me"                                                                        
```                                                                                                                                  
                                                                                                                                     
Respuesta esperada (200): JSON con datos del usuario. El campo id confirma autenticación exitosa.                                    
                                                                                                                                     
Respuesta de error (401):                                                                                                            
                                                                                                                                     
```json                                                                                                                              
  {"error": "Invalid or Inactive Token", "is_authenticated": "False", "status_code": 401}                                            
```                                                                                                                                  
                                                                                                                                     
### check_session() — Verificar que el usuario tiene sesión activa                                                                   
                                                                                                                                     
Ejecuta verify_token(). Si es exitoso, confirma al usuario que la sesión está activa e incluye:                                      
- Nombre completo (first_name + last_name)                                                                                           
- Email                                                                                                                              
- Número de cohortes activas                                                                                                         
                                                                                                                                     
Si falla, usa report_expired().                                                                                                      
                                                                                                                                     
### report_expired() — Guiar al usuario a renovar                                                                                    
                                                                                                                                     
Cuando el token es 401/403:                                                                                                          
                                                                                                                                     
1. Informa claramente: "El token de 4Geeks Academy ha expirado o no es válido."                                                      
2. Indica al usuario que genere uno nuevo en: https://api-token.breathecode.io                                                       
3. Pide que te envíe el nuevo token por este chat para guardarlo en .env.                                                            
4. No intentes operaciones con el token actual — todas fallarán.                                                                     
                                                                                                                                     
Flujo de integración con otras skills                                                                                                
                                                                                                                                     
Otras skills (como 4geeks-connect) deben llamar a verify_token() antes de ejecutar cualquier operación:                              
                                                                                                                                     
```                                                                                                                                  
  ┌─────────────┐     ┌─────────────┐     ┌──────────────┐                                                                           
  │ 4geeks-auth │ ◄── │ Otra skill  │ ◄── │  Pregunta    │                                                                           
  │ verify_token│     │ (connect,   │     │  del usuario │                                                                           
  │ check_session│    │  tasks... ) │     │              │                                                                           
  └─────────────┘     └─────────────┘     └──────────────┘                                                                           
```                                                                                                                                  
                                                                                                                                     
### Protocolo para otras skills                                                                                                      
                                                                                                                                     
Si otra skill necesita verificar autenticación, debe hacer lo siguiente:                                                             
                                                                                                                                     
1. Ejecutar el comando verify_token().                                                                                               
2. Si HTTP 200: continuar con su operación.                                                                                          
3. Si HTTP 401/403: detenerse y delegar en report_expired() — no continuar.                                                          
                                                                                                                                     
No modifiques este flujo sin actualizar esta skill.                                                                                  
                                                                                                                                     
Uso directo desde conversación                                                                                                       
                                                                                                                                     
Cuando el usuario pregunte "¿está funcionando el token?", "¿sigue activa mi sesión?", "verifica mi autenticación":                   
                                                                                                                                     
1. Ejecuta verify_token() mediante exec.                                                                                             
2. Interpreta el código de estado HTTP.                                                                                              
3. Si 200: confirma que el token es válido y la sesión está activa, mostrando nombre y email.                                        
4. Si 401/403: ejecuta report_expired().                                                                                             
                                                                                                                                     
Notas                                                                                                                                
                                                                                                                                     
- Esta skill NO consulta perfiles, cohortes, tareas, certificados ni actividad.                                                      
- Esta skill NO almacena ni cachea el token.                                                                                         
- Esta skill NO hace llamadas POST, PUT, PATCH ni DELETE.                                                                            
- El token se guarda en .env del workspace, no debe leerse directamente.                                                             
```                                                              