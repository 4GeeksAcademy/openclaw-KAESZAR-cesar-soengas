  # 4geeks-certificates — Certificados obtenidos                                                                                     
                                                                                                                                     
  ## Propósito                                                                                                                       
                                                                                                                                     
  Consultar los certificados emitidos al estudiante en 4Geeks Academy / BreatheCode.                                                 
  Responsabilidad única: listar y mostrar certificados obtenidos.                                                                    
                                                                                                                                     
  ## Límites                                                                                                                         
                                                                                                                                     
  - Solo lectura (`GET`). **Prohibido** enviar `POST`, `PUT`, `PATCH` o `DELETE`.                                                    
  - No mezclar autenticación, perfil, tareas, proyectos ni otra información.                                                         
  - Ante un `401` o `403` que no sea de permisos, informa al usuario que el token expiró y pídele que genere uno nuevo en            
https://api-token.breathecode.io.                                                                                                    
  - Si el endpoint devuelve lista vacía, informa que no hay certificados emitidos aún.                                               
                                                                                                                                     
  ## Uso                                                                                                                             
                                                                                                                                     
  Usa `python3` para procesar JSON (`jq` no disponible por defecto en el workspace).                                                 
                                                                                                                                     
  Activa el venv si existe (`source /root/.openclaw/workspace/.venv/bin/activate`) para usar python3.                                
                                                                                                                                     
  La base URL de la API es:                                                                                                          
                                                                                                                                     
```                                                                                                                                  
                                                                                                                                     
https://breathecode.herokuapp.com                                                                                                    
                                                                                                                                     
```                                                                                                                                  
                                                                                                                                     
  Token de API configurado en `/root/.openclaw/workspace/.env` como `TOKEN_4GEEKS`.                                                  
  Cárgalo en una variable para no exponer el valor:                                                                                  
                                                                                                                                     
  ```sh                                                                                                                              
  TOKEN_4GEEKS=$(grep ^TOKEN_4GEEKS /root/.openclaw/workspace/.env | cut -d= -f2)                                                    
```                                                                                                                                  
                                                                                                                                     
### Endpoint                                                                                                                         
                                                                                                                                     
```                                                                                                                                  
  GET /v1/certificate/me                                                                                                             
```                                                                                                                                  
                                                                                                                                     
No requiere header Academy. Devuelve un array de certificados emitidos al estudiante autenticado.                                    
                                                                                                                                     
### Ejemplo completo                                                                                                                 
                                                                                                                                     
```sh                                                                                                                                
  TOKEN_4GEEKS=$(grep ^TOKEN_4GEEKS /root/.openclaw/workspace/.env | cut -d= -f2)                                                    
  curl -s -H "Authorization: Token ${TOKEN_4GEEKS}" \                                                                                
    "https://breathecode.herokuapp.com/v1/certificate/me" \                                                                          
    | python3 -c "                                                                                                                   
  import sys, json                                                                                                                   
  data = json.load(sys.stdin)                                                                                                        
  for c in data:                                                                                                                     
      print(f\"- {c['specialty']['name']} | emitido: {c['issued_at'][:10]}\")                                                        
      if c.get('preview_url'):                                                                                                       
          print(f\"  preview: {c['preview_url']}\")                                                                                  
      if c.get('pdf_url'):                                                                                                           
          print(f\"  pdf: {c['pdf_url']}\")                                                                                          
  "                                                                                                                                  
```                                                                                                                                  
                                                                                                                                     
### Campos relevantes por certificado                                                                                                
                                                                                                                                     
┌────────────────┬────────────────────────────────────────────────────────────────────────────────────┐                              
│ Campo          │ Descripción                                                                        │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ specialty.name │ Nombre de la especialidad/módulo                                                   │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ specialty.slug │ Slug de la especialidad                                                            │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ issued_at      │ Fecha de emisión                                                                   │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ signed_by      │ Instructor que firmó                                                               │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ signed_by_role │ Rol de quien firmó                                                                 │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ status         │ Estado (PERSISTED, etc.)                                                           │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ status_text    │ Texto descriptivo del estado                                                       │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ preview_url    │ URL de previsualización de la imagen del certificado                               │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ pdf_url        │ URL para descargar el PDF (Nota: puede ser https://certificate.4geeks.com/pdf/...) │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ academy.name   │ Nombre de la academia                                                              │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ cohort.name    │ Nombre de la cohorte asociada                                                      │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ cohort.slug    │ Slug de la cohorte                                                                 │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ layout.slug    │ Layout usado (default)                                                             │                              
├────────────────┼────────────────────────────────────────────────────────────────────────────────────┤                              
│ expires_at     │ Fecha de expiración (null si no expira)                                            │                              
└────────────────┴────────────────────────────────────────────────────────────────────────────────────┘                              
                                                                                                                                     
### Formateo de ejemplo para output al usuario                                                                                       
                                                                                                                                     
```                                                                                                                                  
  Certificados obtenidos (5):                                                                                                        
                                                                                                                                     
  1. 🎓 Command Line, Git and Github — emitido 2026-10-14                                                                            
     📄 https://storage.googleapis.com/certificates-breathecode/5bd26a0...                                                           
                                                                                                                                     
  2. 🎓 Web UI fundamentals with Tailwind — emitido 2026-10-03                                                                       
                                                                                                                                     
  3. 🎓 Basic personal assistants with Openclaw — emitido 2026-10-03                                                                 
                                                                                                                                     
  4. 🎓 No-Code & Automations — emitido 2026-09-23                                                                                   
                                                                                                                                     
  5. 🎓 Working with AI coding agents — emitido 2026-09-19                                                                           
```                                                                                                                                  
                                                                                                                                     
Respuestas ante errores                                                                                                              
                                                                                                                                     
- 401/403 sin mensaje de permiso: Token inválido/inactivo. Pedir nuevo token en https://api-token.breathecode.io.                    
- 403 con don't have this capability: read_certificate: El usuario no tiene permiso read_certificate. Informar que la API no permite 
  consultar certificados para esta cuenta.                                                                                           
- Lista vacía ([]): No hay certificados emitidos aún.                                                                                
- Error de conexión o DNS: Informar que la API de BreatheCode no está disponible temporalmente.