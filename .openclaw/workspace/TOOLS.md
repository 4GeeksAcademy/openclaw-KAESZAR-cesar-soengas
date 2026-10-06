```sh                                                                                                                                  
  cat > /root/.openclaw/workspace/TOOLS.md <<'EOF'                                                                                     
  # TOOLS.md - Servicios y valores predeterminados                                                                                     
                                                                                                                                       
  Este archivo documenta los servicios solicitados y las reglas prácticas para usarlos. El estado de conexión se anota solo cuando está
verificado; la presencia de una herramienta general no demuestra que una cuenta esté conectada.                                        
                                                                                                                                       
  ## Google Calendar                                                                                                                   
                                                                                                                                       
  - **Estado en esta sesión:** no hay una herramienta de Google Calendar disponible en el catálogo actual. La conexión de una cuenta y 
sus calendarios no está verificada; no afirmar que se consultó o modificó un calendario.                                               
  - **Cuándo usarlo:** consultar agenda, buscar disponibilidad y crear o modificar eventos cuando el usuario lo pida y la integración  
esté disponible.                                                                                                                       
  - **Valores predeterminados:**                                                                                                       
    - Respetar la zona horaria configurada en el calendario. Si no se puede consultar, pedir o confirmar la zona horaria antes de      
programar; no asumir UTC para citas locales.                                                                                           
    - Mantener título, fecha, horario, participantes y ubicación tal como los indique el usuario. Preguntar por cualquier dato faltante
que pueda cambiar la cita.                                                                                                             
    - No invitar participantes, añadir videollamadas ni configurar recordatorios si no se solicitaron.                                 
    - Confirmar antes de eliminar un evento o de hacer un cambio ambiguo que afecte a otras personas.                                  
    - Después de una acción, verificar el resultado y comunicar fecha, hora y zona horaria.                                            
                                                                                                                                       
  ## Telegram                                                                                                                          
                                                                                                                                       
  - **Estado en esta sesión:** están disponibles herramientas generales para mensajes en canales compatibles, pero la búsqueda de      
conversaciones de Telegram no devolvió destinos. No hay un chat o destinatario de Telegram verificado para esta sesión.                
  - **Cuándo usarlo:** enviar o gestionar mensajes de Telegram solo cuando el usuario solicite esa acción, el canal esté conectado y el
destinatario o conversación se identifique sin ambigüedad.                                                                             
  - **Valores predeterminados:**                                                                                                       
    - No adivinar destinatarios, grupos, canales ni IDs. Si no hay un destino verificado, pedir al usuario que conecte Telegram o      
especifique uno disponible.                                                                                                            
    - Antes de enviar, respetar literalmente el contenido y la audiencia indicados; no añadir datos privados ni destinatarios no       
solicitados.                                                                                                                           
    - Usar texto breve en español si el usuario escribe en español; preferir listas sencillas y evitar tablas.                         
    - No enviar archivos, imágenes, mensajes programados ni publicaciones a grupos/canales salvo petición explícita.                   
    - Informar si el mensaje quedó enviado, en cola o si no se pudo verificar su entrega.                                              
                                                                                                                                       
  ## Regla general                                                                                                                     
                                                                                                                                       
  - Verificar conexión, cuenta y destino en las herramientas disponibles antes de decir que un servicio está conectado o antes de      
actuar sobre él.                                                                                                                       
  - Si una operación no está disponible o falta un dato esencial, explicar el límite concreto y preguntar solo lo necesario. Nunca     
inventar permisos, conexiones ni resultados.                                                                                           
  EOF                                                                                                                                  
```  