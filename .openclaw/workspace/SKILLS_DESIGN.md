                                                                                                                                                                                                                      
  # Diseño de skills                                                                                                                                       
                                                                                                                                                           
  Antes de escribir o implementar una skill, documenta aquí su diseño. Completa una sección por cada skill que se vaya a implementar. No empieces a        
construirla hasta responder las tres preguntas.                                                                                                            
                                                                                                                                                           
  ## Plantilla por skill                                                                                                                                   
                                                                                                                                                           
  ### Nombre de la skill: [nombre]                                                                                                                         
                                                                                                                                                           
  **¿Qué hace?**                                                                                                                                           
  [Descríbelo en una sola frase.]                                                                                                                          
                                                                                                                                                           
  **¿Qué input necesita el agente?**                                                                                                                       
  [Especifica qué información se proporciona y en qué formato. Indica también qué datos ya conoce el agente a partir de los cinco archivos de configuración
y cuáles debe pedir o verificar.]                                                                                                                          
                                                                                                                                                           
  **¿Cómo es un buen output?**                                                                                                                             
  [Describe el formato esperado, el destino —por ejemplo, un Doc, un evento de Calendar o un mensaje de Telegram— y cómo se verificará que la operación    
funcionó.]                                                                                                                                                 
                                                                                                                                                           
  ---                                                                                                                                                      
                                                                                                                                                           
                               
### Nombre de la skill: 4geeks-academy

**¿Qué hace?**  
Consulta información educativa del propio estudiante en la API de 4Geeks Academy/BreatheCode, como perfil, cohortes, tareas, certificados, actividad, contenidos y eventos, sin modificar datos.

**¿Qué input necesita el agente?**  
La consulta del usuario y, cuando corresponda, filtros como estado/tipo de tarea, cohorte, rango de fechas, tecnología o dificultad. Puede descubrir la academia desde `profile_academy` y las cohortes desde la respuesta de `/v1/admissions/academy/cohort/me`; no debe asumir una academia o cohorte si hay más de una opción válida. `TOKEN_4GEEKS` está configurada como referencia de entorno, pero se debe verificar cómo se obtiene y si es un token API válido antes de usarla. La base URL predeterminada es `https://breathecode.herokuapp.com`; debe confirmarse si el usuario indica otra instancia. Solo puede realizar llamadas si hay una herramienta API autorizada y disponible.

**¿Cómo es un buen output?**  
Una respuesta breve en español, limitada a los datos necesarios para responder, con estados y fechas tal como los devuelve la API. Debe distinguir entre datos consultados y datos no disponibles, y no afirmar que accedió a la plataforma si no hubo una respuesta verificable de la herramienta. Esta primera versión solo hace consultas GET; no actualiza ni entrega tareas.

---

