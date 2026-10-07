  Este archivo documenta los servicios solicitados y las reglas prácticas para usarlos. El estado de conexión se anota solo cuando está    
verificado; la presencia de una herramienta general no demuestra que una cuenta esté conectada.                                            
                                                                                                                                           
  ## Google Calendar                                                                                                                       
                                                                                                                                           
  - **Skill OpenClaw:** `google-calendar` está creada e instalada como skill (`~/.openclaw/workspace/skills/google-calendar/SKILL.md`); su 
fuente está en `.agents/skills/google-calendar/SKILL.md` dentro del repositorio del proyecto.                                              
  - **Estado del servicio:** Google Calendar aún no está conectado ni autorizado en esta sesión. En la última comprobación no estaba       
disponible el binario `gog`, requisito de la integración documentada por la skill. La skill enseña el flujo seguro, pero no proporciona por
sí misma acceso a la cuenta ni permite operar el calendario.                                                                               
  - **Cuándo usarlo:** consultar agenda, buscar disponibilidad y crear o actualizar eventos cuando el usuario lo pida y una integración    
autorizada esté disponible. Si la integración sigue sin estar disponible, explicar el bloqueo y no afirmar que se consultó o modificó      
Calendar.                                                                                                                                  
  - **Valores predeterminados:**                                                                                                           
    - Respetar la zona horaria del calendario; si no se puede consultar y la zona afecta la cita, preguntar antes de programar. No asumir  
UTC para citas locales.                                                                                                                    
    - Para crear o actualizar, confirmar los datos ausentes o ambiguos que cambien el resultado: calendario, evento, fecha, inicio/fin,    
zona horaria y participantes.                                                                                                              
    - No añadir invitados, videollamadas, recordatorios, recurrencia ni ubicación salvo que se solicite.                                   
    - No borrar eventos sin una instrucción explícita y un objetivo inequívoco.                                                            
    - Tras una operación, verificar el resultado de la herramienta y comunicar el horario y estado confirmados.                            
                                                                                                                                           
  ## Telegram                                                                                                                              
                                                                                                                                           
  - **Skill OpenClaw:** `telegram-messaging` está creada e instalada como skill                                                            
(`~/.openclaw/workspace/skills/telegram-messaging/SKILL.md`); su fuente está en `.agents/skills/telegram-messaging/SKILL.md` dentro del    
repositorio del proyecto.                                                                                                                  
  - **Estado verificado (2026-10-07):** canal predeterminado habilitado, configurado, en ejecución y conectado mediante polling. Bot:      
`@Jazz_cesar_bot`. El emparejamiento del usuario está aprobado. La credencial está guardada como secreto; nunca mostrarla ni copiarla.     
  - **Cuándo usarlo:** redactar, buscar conversaciones o enviar mensajes cuando el usuario lo solicite. Distinguir “redacta/prepara” de    
“envía”; pedir un borrador no autoriza su envío.                                                                                           
  - **Valores predeterminados:**                                                                                                           
    - Para resolver el destino, consultar `conversations_list` con `channel: "telegram"` y utilizar solo un `conversationRef` devuelto para
`conversations_send`. No adivinar chats, personas, grupos, canales ni IDs; si la coincidencia no es inequívoca, preguntar.                 
    - Enviar solo con instrucción explícita para el mensaje y destino. Que el bot esté conectado o el usuario emparejado no autoriza       
mensajes a otros chats ni publicaciones generales.                                                                                         
    - Preservar el contenido y audiencia indicados; no añadir datos privados, destinatarios ni archivos no solicitados.                    
    - Si el usuario solo pide redactar, devolver el texto en el chat actual sin enviarlo por Telegram.                                     
    - Usar texto breve en español si el usuario escribe en español; preferir listas sencillas y evitar tablas.                             
    - Informar exactamente el estado de envío (enviado, en cola, suprimido, desconocido o fallido). No llamar “entregado” a un envío no    
confirmado; ante estado desconocido, comprobar antes de reintentar para evitar duplicados.                                                 
                                                                                                                                           
  ## Regla general                                                                                                                         
                                                                                                                                           
  - Verificar conexión, cuenta y destino en las herramientas disponibles antes de decir que un servicio está conectado o antes de actuar   
sobre él.                                                                                                                                  
  - Si una operación no está disponible o falta un dato esencial, explicar el límite concreto y preguntar solo lo necesario. Nunca inventar
permisos, conexiones ni resultados. 