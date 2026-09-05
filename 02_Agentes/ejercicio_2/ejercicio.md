# Ejercicio 2 — Descripción PEAS de agentes inteligentes

### 1. Asistente virtual de voz

- **Performance:** % de instrucciones entendidas correctamente, % de tareas ejecutadas correctamente, tiempo de respuesta, tiempo en ejecutar la acción
- **Environment:** Totalmente observable, estocástico, episódico, dinámico y continuo.
- **Actuators:** Hablar, encender el A/C, poner música
- **Sensors:** Micrófono, reloj, dispositivos vinculados, botones

**Justificación del entorno:**
Diría que parcialmente observable puesto que a pesar de que el agente cuenta con información necesaria para resolver las tareas, como recibir el audio y la instrucción, puede que esta esté relacionada a elementos de la casa como luces y cosas así que puede que no siempre sean observables. Es estocástico porque muchas veces hay ruido de fondo, acentos, o la forma en la que habla un usuario puede no ser la misma siempre, entonces no siempre va a ser la misma entrada, y la salida igual sería diferente. Es episódico porque se activaría cada que se hace una consulta con el usuario, cada vez que tire un comando de A/C. Es dinámico porque el mundo sigue cambiando; si por ejemplo alguien pone música desde un dispositivo y alguien más lo puso en otro, el asistente debe ser capaz de reaccionar a eso. Es continuo porque involucra entradas de audio y devolver una salida de audio, que lo haría más cercano a algo continuo.


### 2. Robot aspirador doméstico

- **Performance:** % de espacios sucios determinados correctamente, % de limpieza en un punto, número de veces que se cicla, % de limpieza efectiva en un piso completo, # de rutas repetidas erróneamente
- **Environment:** Totalmente observable, estocástico, secuencial, dinámico y continuo.
- **Actuators:** Avanzar, girar, succionar, ir a la base
- **Sensors:** Sensor de choque, sensor de polvo, ruedas, cámara

**Justificación del entorno:**
Lo consideré totalmente observable puesto que el robot aspirador debería tener los sensores que le permitan descubrir si necesita no limpiar una casilla, ya sea a su derecha, a su izquierda, o moverse; entonces sí tiene todo lo que necesita para poder decidir qué acción tomar. Es estocástico porque el tipo de basura siempre es diferente: hay distintas formas de ubicar un mueble y pueden cambiar esa clase de cosas, entonces sí hay cierto factor no determinista. Es secuencial puesto que primero debe limpiar por secciones y, al limpiar algunas, eso le permitiría liberar otras, pero al mismo tiempo debería optimizarlo para no pasar tanto por lugares que ya limpió. Es dinámico porque puede que llegue basura nueva después y debería poder detectarla, sin perder tanto tiempo buscando basura en lugares que ya limpió, sino detectarlo adecuadamente; y si nota que cayó algo más, limpiarlo. Es continuo, no discreto, porque la forma en la que se involucran los sensores para leer si hay o no basura, y para saber si limpiar o no, no se reduce a casillas fijas.


### 3. Sistema de recomendación de streaming

- **Performance:** % de efectividad de abrir la recomendación, # de minutos vistos de cada recomendación, # de recomendaciones acertadas, tiempo que se pasa leyendo la recomendación
- **Environment:** totalmente observable, estocástico, secuencial, estático y continuo.
- **Actuators:** Mostrar/ordenar recomendaciones (películas o canciones), reproducir automáticamente la siguiente
- **Sensors:** Historial del usuario, pausas, plays, calificaciones

**Justificación del entorno:**
Lo consideré totalmente observable puesto que, para algún usuario y para generar su sistema de recomendaciones, podríamos acceder a sus visualizaciones y a los clics que ha hecho; toda esa información sí podría estar almacenada, a pesar de que puede parecer mucha información, entonces puede llegar a ser totalmente observable. Es estocástico puesto que hay cosas circunstanciales al hacer clic o no en una recomendación o en alguna película que podría gustarnos o no: eso no determinaría siempre que le gustó o no una película, sino que hay distintos escenarios. Es secuencial puesto que las películas que vea de las recomendaciones y a las que les dé like también van a afectar en futuras recomendaciones. En general diría que es estático, en mayor medida, mientras el agente está tomando la decisión: al generar la recomendación podría calcularse sin necesidad de que el usuario interactúe con el mundo; sin embargo, sí podría ser el caso en donde, mientras se está calculando la recomendación, el usuario abre otra cosa y eso podría afectarlo. Finalmente es continuo, puesto que hay distintas formas de valorar una película: saber si le gustó o no no son solo las estrellas que le puso, sino también el tiempo que la vio, la cantidad de veces, la cantidad de minutos, etcétera.


### 4. Vehículo autónomo en ciudad

- **Performance:** # de choques, % de objetos identificados correctamente, % de decisiones correctas al maniobrar, % de rutas óptimas elegidas
- **Environment:** Parcialmente observable, estocástico, secuencial, dinámico y continuo.
- **Actuators:** Girar, acelerar, frenar, encender intermitentes, tocar el claxon
- **Sensors:** Sensor de proximidad, cámara, radar, GPS, micrófono

**Justificación del entorno:**
Lo consideré parcialmente observable puesto que hay ángulos a los que probablemente aún no tienen acceso. Sin embargo, es de las cosas que más deben buscarse optimizar, puesto que entre mejor acceso tengan a su entorno, más seguro se van a volver. Sí hay acciones impredecibles que necesitarían sensores muy desarrollados para poder ser observables. Es estocástico, definitivamente, puesto que el mundo real en una ciudad, en el tráfico, puede variar bastante, entonces necesita poder reaccionar ante los distintos posibles escenarios, que pueden ser infinitos. Es secuencial puesto que conducir por una calle o elegir una ruta puede afectar a lo que viene después; si hay mucho tráfico, entonces tienen que tomarse decisiones seguras también para el coche. Es dinámico porque, mientras se está manejando, el mundo está cambiando: la gente puede atravesarse, puede surgir un coche de la nada, entonces debes saber reaccionar a eso. Finalmente es continuo, puesto que hay distintos sensores involucrados, como el de movimiento y el de la velocidad, que tienen valores continuos.


### 5. Agente de trading algorítmico en bolsa

- **Performance:** Profit promedio por inversión, número de inversiones acertadas, dinero perdido, % de ganancias totales
- **Environment:** parcialmente observable, estocástico, secuencial, dinámico y continuo.
- **Actuators:** Comprar, vender, cancelar una orden, mantener la posición
- **Sensors:** Valores actuales, transacciones, reloj

**Justificación del entorno:**
Lo consideré parcialmente observable puesto que, en la bolsa, sí debe haber cosas que son impredecibles: más allá de que se pueden tomar datos históricos en muchos sentidos, hay decisiones que pueden afectar a grandes empresas, que pueden afectar bastante, y que no son observables. Es estocástico puesto que, al tratarse de agentes que pueden estar al tanto de acciones de muchas empresas, todo esto trae azar y efectos que no se pueden controlar dentro del entorno. Es secuencial puesto que las acciones en las que se decide invertir o no invertir van a tener efectos después, y no son solo casos aislados. Es dinámico puesto que, mientras se compra o mientras se decide si se va a comprar o no, están cambiando los valores. Es continuo puesto que, al tratarse de valores dentro de los números reales en el trading algorítmico, no se trata para nada de valores discretos los que se están tomando en cuenta.


### 6. Sistema de diagnóstico médico asistido por IA

- **Performance:** % de diagnósticos correctos, # de falsos positivos / negativos, tiempo de respuesta, nivel de especificidad en el diagnóstico
- **Environment:** Parcialmente observable, estocástico, episódico, dinámico y continuo.
- **Actuators:** Mostrar un diagnóstico sugerido, marcar zonas en una imagen, sugerir más estudios, generar un reporte
- **Sensors:** Rayos X, signos vitales, cámaras

**Justificación del entorno:**
Lo consideré parcialmente observable puesto que, más allá de que se pueda configurar con los sensores para la mayoría de cosas del cuerpo, siempre habrá algún factor que se pueda ignorar; entonces no diría que puede llegar a ser totalmente observable, o al menos sería muy difícil. Es estocástico puesto que, de la misma forma, el cuerpo humano termina por tener cierto comportamiento impredecible, así que un sistema de diagnóstico médico también va a tener este carácter estocástico. Es episódico puesto que aquí sí se podrían hacer consultas aisladas: en determinado momento, con determinada entrada y con determinado estado del usuario, poder hacer un diagnóstico específico para ese momento. Es dinámico, definitivamente, puesto que mientras se procesen los resultados puede que el cuerpo vaya cambiando, entonces hay que estar monitoreando constantemente; un buen sistema de diagnóstico lo haría. Es continuo puesto que hay demasiadas variables que tendrían que entrar dentro de los números reales, como temperatura del cuerpo, peso, etcétera, sobre todo si se quiere ser más específico.


### 7. Dron de inspección de infraestructura

- **Performance:** % de fugas o fallas detectadas, % de rutas repetidad innecesariamente, % de choques, duración de la batería
- **Environment:** Totalmente observable, estocástico, secuencial, dinámico y continuo.
- **Actuators:** Subir, bajar, desplazarse, tomar foto o video, aterrizar
- **Sensors:** Cámara, sensor de movimiento, micrófono, GPS, termómetro

**Justificación del entorno:**
Lo consideré totalmente observable, hasta cierto punto, si es posible identificar todas las zonas con sensores: hacia dónde se puede mover el dron, si debe subir, si va a ser por tierra o por aire, y hacia dónde se debe mover. Es estocástico porque sí hay factores que no son totalmente determinados o concretos, sino que puede haber cosas que no estén necesariamente controladas en este entorno. Es secuencial puesto que las rutas de inspección o las decisiones que tomes sí afectan después al proceso entero y a la toma de decisiones del flujo completo. Es dinámico puesto que la infraestructura o la maquinaria probablemente seguiría cambiando, el estado seguiría cambiando. Finalmente es continuo puesto que muchos de esos atributos también terminan por caer en los números reales, y no son tan específicos como para caer dentro de lo discreto.


### 8. Agente jugador de ajedrez

- **Performance:** % de victorias, tiempo de movimiento, % de jugadas óptimas decididas, % de jaque mates identificados
- **Environment:** Totalmente observable, determinista, secuencial, estático y discreto.
- **Actuators:** Hacer un movimiento, ofrecer tablas, rendirse
- **Sensors:** Reloj, posición del tablero

**Justificación del entorno:**
Lo consideré totalmente observable puesto que el tablero de ajedrez es totalmente visible. Es determinista puesto que, para una entrada, siempre hay una mejor decisión o un conjunto de decisiones que podrían calcularse, más allá de que tome millones de cálculos o de movimientos posibles; al final me parece que sí cae de esa manera. Es secuencial puesto que las decisiones de un movimiento terminan afectando al futuro de la partida. Puede llegar a ser estático si es configurado para pensar únicamente después de que el otro jugador mueve. Finalmente es discreto puesto que las casillas y las jugadas se pueden contabilizar dentro de los números enteros.
