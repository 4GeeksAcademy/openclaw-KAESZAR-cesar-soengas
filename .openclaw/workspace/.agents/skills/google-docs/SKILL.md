 name: google-docs                                                                                                                           
  description: Consulta y gestiona documentos de Google Docs cuando el usuario solicite buscar, leer, exportar o copiar un documento; confirma
el documento objetivo y la operación antes de crear o modificar nada.                                                                         
  ---                                                                                                                                         
                                                                                                                                              
  # Google Docs                                                                                                                               
                                                                                                                                              
  Utiliza esta habilidad para buscar, leer y copiar documentos de Google Docs (Drive). Describe cómo el agente debe utilizar una integración  
de Docs/Drive autorizada y disponible; por sí misma no instala `gog`, no concede acceso a Google ni autoriza OAuth.                           
                                                                                                                                              
  ## Límites                                                                                                                                  
                                                                                                                                              
  - La vía prevista es la integración `gog` de OpenClaw (comandos `gog drive search`, `gog docs cat`, `gog docs export`, `gog docs copy`).    
Requiere el binario `gog` instalado y una cuenta OAuth autorizada con los servicios `docs` y `drive`.                                         
  - `gog` no edita el contenido de un documento en el lugar: solo busca, lee, exporta y copia. Para crear o editar contenido hace falta un    
cliente de la Docs API autorizado; si no está disponible, dilo y detente.                                                                     
  - No inventes comandos ni flags. Si necesitas una operación no documentada aquí, consulta `gog docs --help` o `gog drive --help` en el      
entorno antes de ejecutarla.                                                                                                                  
                                                                                                                                              
  ## Flujo de trabajo                                                                                                                         
                                                                                                                                              
  1. **Verifica la disponibilidad de la herramienta.** Comprueba que `gog` está instalado y autorizado para Docs/Drive (por ejemplo, `gog auth
list` o `gog auth doctor`; no te fíes solo del código de salida). Si falta la herramienta o la autorización, informa el impedimento exacto y  
detente; no inventes una integración alternativa ni afirmes acceso a Docs.                                                                    
  2. **Clasifica la solicitud.** Distingue entre buscar un documento, leer/exportar su contenido y copiarlo. Una petición de redactar texto no
equivale a una petición de crear o modificar un documento.                                                                                    
  3. **Identifica el documento.** Usa la URL o el ID (docId) que el usuario proporcione. Si no lo da, busca con `gog drive search "<consulta>"
--max 10` y confirma con el usuario la coincidencia inequívoca (título y URL) antes de operar. Nunca actúes sobre un documento meramente      
similar.                                                                                                                                      
  4. **Lee o exporta.** Para el contenido usa `gog docs cat <docId>`; cuando haga falta un archivo, `gog docs export <docId> --format txt     
--out <ruta>`. Verifica que el comando devuelva contenido o archivo válido.                                                                   
  5. **Copia (solo si se solicita).** Usa el comando de copia de `gog docs` (confirma la sintaxis exacta con `gog docs --help` en el entorno) 
y verifica el documento creado por su título y URL devueltos.                                                                                 
  6. **Verifica e informa.** Resume el documento identificado (título y URL/ID) y el resultado: contenido leído, archivo exportado con su     
ruta, o copia creada con su enlace. Distingue siempre lo obtenido de la herramienta de lo que no se pudo verificar; no afirmes que accediste a
Docs sin una respuesta comprobable.                            