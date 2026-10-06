```sh
mkdir -p /root/.openclaw/workspace/openclaw-KAESZAR-cesar-soengas/.agents/skills/telegram-messaging

cat > /root/.openclaw/workspace/openclaw-KAESZAR-cesar-soengas/.agents/skills/telegram-messaging/SKILL.md <<'EOF'
---
name: telegram-messaging
description: Redactar, buscar y enviar mensajes de Telegram cuando el usuario lo solicite, especialmente al elegir un destino verificado, redactar una respuesta o confirmar la entrega. 
---

# Mensajería de Telegram

Utiliza esta habilidad para la redacción y el envío de mensajes de Telegram. El canal de Telegram de OpenClaw está conectado, pero la habilidad debe verificar que el destino solicitado esté disponible y nunca debe adivinar un chat, grupo, canal o identificador. 

## Flujo de trabajo

1. **Clasificar la solicitud.** Determina si el usuario desea un borrador, quiere buscar/leer una conversación o da instrucciones explícitas para enviar un mensaje. Una solicitud para redactar o discutir un texto no constituye un permiso para enviarlo. 
2. **Verificar la integración cuando se solicite la entrega o búsqueda.** Utiliza las herramientas de conversación y mensajería de OpenClaw disponibles. Para un destino exacto y conocido, utiliza `conversations_list` con `channel: "telegram"` y una consulta útil si es necesario; utiliza únicamente el `conversationRef` devuelto con `conversations_send`. El bot de Telegram conectado es `@Jazz_cesar_bot`, pero ese hecho por sí solo no identifica al destinatario o la conversación previstos. 
3. **Resolver el destino.** Haz coincidir la persona, chat, grupo o canal especificado por el usuario con una conversación de Telegram devuelta. Si no existe una coincidencia inequívoca, detente y pregunta qué destino utilizar. Nunca infieras un destinatario a partir de un chat no relacionado, la similitud de nombres de usuario o el hecho de que haya una cuenta de Telegram vinculada. 
4. **Preparar el contenido.** Preserva la intención y la audiencia del usuario. Escribe un texto conciso y natural en el idioma solicitado; de lo contrario, utilice el idioma de la solicitud del usuario. Dé prioridad a un formato sencillo frente a las tablas. No añada detalles privados, afirmaciones, archivos adjuntos ni destinatarios que el usuario no haya solicitado. 
5. **Deténgase ante ambigüedades sustanciales.** Formule una pregunta breve si el mensaje, el destinatario o algún detalle relevante no están claros. No realice el envío hasta que se haya aclarado la situación. 
6. **Envíe solo cuando se solicite explícitamente.** Si la solicitud es únicamente para un borrador, devuélvalo a través del chat sin utilizar la herramienta de envío. Para un envío explícito, remita el texto revisado al destino correspondiente utilizando los valores exactos de `conversationRef` y `conversations_send` obtenidos previamente. 
7. **Verifique e informe.** Considere el estado `status: "sent"` como la confirmación de envío de la herramienta. Informe los estados de cola (*queued*), supresión (*suppressed*), desconocido (*unknown*) o fallo (*failed*) tal cual son; nunca describa una entrega no confirmada como enviada. No reintente un envío ambiguo o desconocido sin realizar comprobaciones previas, para evitar mensajes duplicados.