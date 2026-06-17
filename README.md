# TPFINAL_Grupo3
Sistema automático de dosificación de fluido con sensor de distancia

1. DESCRIPCIÓN GENERAL DEL PROYECTO
El proyecto consiste en una maqueta funcional de un sistema automático de dosificación y control de llenado de un recipiente. El sistema mide el nivel de líquido mediante un sensor de distancia infrarrojo Sharp, cuya salida analógica es leída por el ADC del PIC16F887. A partir de esa medición, el microcontrolador determina el porcentaje aproximado de llenado y lo muestra en un display de 7 segmentos multiplexado.
Como actuador se utiliza una mini bomba de agua controlada mediante un MOSFET. Además, se incorpora un botón de emergencia mediante interrupción externa, que permite detener el funcionamiento del sistema, apagar la bomba y activar una indicación visual de falla mediante un LED y el mensaje “FAIL” en el display.

2. Alcances del Proyecto
El sistema SÍ es capaz de:
Medir una señal analógica proveniente de un sensor de distancia Sharp.
Leer la señal del sensor mediante el ADC interno del PIC16F887.
Interpretar la distancia medida como un nivel aproximado de llenado del recipiente.
Mostrar el estado del sistema en un display de 7 segmentos multiplexado.
Mostrar una escala aproximada de porcentaje de llenado.
Controlar una mini bomba de agua mediante una salida digital conectada al gate de un MOSFET.
Activar un estado de emergencia mediante un pulsador conectado a RB0/INT.
Mantener el estado de emergencia hasta una nueva pulsación del botón.
Apagar la bomba ante una emergencia.
Encender un LED de alarma ante una emergencia.
Mostrar el mensaje “FAIL” en el display cuando el sistema está en estado de falla.
Utilizar Timer0 para el refresco periódico del display multiplexado.
Integrar ADC, Timer0, interrupción externa y salidas digitales en un único sistema funcional.

El sistema NO incluye / Fuera de alcance:
Control preciso de caudal en mL/min.
Medición médica real ni uso clínico.
Control PID o regulación avanzada.
Medición directa de presión o caudal.
Almacenamiento local de datos en tarjeta SD o memoria externa.
Conectividad inalámbrica Wi-Fi, Bluetooth o IoT.
Interfaz gráfica en PC o aplicación móvil.
Certificación biomédica, esterilidad o seguridad clínica.
Diseño final en PCB, ya que el prototipo se implementa como maqueta educativa/simulación.

4. Posibles Etapas Siguientes
En una versión mejorada, el sistema podría mejorarse incorporando una medición más precisa del volumen dosificado. Para eso se podría reemplazar o complementar el sensor de distancia con un sensor de caudal, una celda de carga o un sensor de presión calibrado. Esto permitiría estimar con mayor exactitud el volumen real entregado por la bomba.
También podría migrarse el circuito desde protoboard o simulación a un circuito impreso PCB, incorporando mejores prácticas de diseño electrónico, filtrado de alimentación, planos de masa, protecciones contra ruido eléctrico y compatibilidad electromagnética. Además, se podría implementar una interfaz UART más completa para enviar datos a una PC, registrar mediciones, configurar setpoints y visualizar el estado del sistema.
Como mejora adicional, podría utilizarse una bomba peristáltica para obtener una dosificación más controlada. También podrían agregarse alarmas sonoras, almacenamiento, y modos de bajo consumo.

5. Arquitectura del Sistema: Hardware y Software
4.1 Hardware & Interconexión
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/0a332185-7f6d-4fd3-a8ff-c1ce797d5b31" />



4.2 Descripción del circuito y consideraciones de diseño
El circuito utiliza un microcontrolador PIC16F887 como unidad central de procesamiento. El sensor Sharp entrega una señal analógica que depende de la distancia medida. Esta señal se conecta a la entrada AN8/RB2 del PIC16F887, donde es convertida por el ADC interno.
El display de 7 segmentos es de ánodo común y se controla de forma multiplexada. Los segmentos se conectan al PORTD, mientras que la habilitación de cada dígito se realiza mediante RA0, RA1, RA2 y RA3. Como el display es de ánodo común, los segmentos se activan con nivel lógico bajo.
La mini bomba de agua se controla mediante el pin RB7, conectado al gate de un MOSFET. El MOSFET actúa como una llave electrónica, permitiendo manejar la corriente de la bomba sin exigir corriente directamente al pin del PIC. Para proteger el circuito frente a los picos generados por la carga inductiva de la bomba, se debe colocar un diodo en contrafase en paralelo con la bomba. El cátodo del diodo se conecta al positivo de alimentación de la bomba y el ánodo al lado del MOSFET.
El botón de emergencia se conecta a RB0/INT. Al presionarlo, el sistema conmuta el estado de emergencia: detiene la bomba, enciende el LED de alarma conectado a RB4 y muestra “FAIL” en el display. Al volver a presionarlo, el sistema sale del estado de emergencia.

4.3 Asignación de pines
RB0 / INT: Botón de emergencia
RB2 / AN8: Entrada analógica del sensor Sharp
RB4: LED amarillo de alarma
RB7: Control de bomba mediante MOSFET
PORTD: Segmentos del display de 7 segmentos
RA0: Habilitación display 1
RA1: Habilitación display 2
RA2: Habilitación display 3
RA3: Habilitación display 4
VDD: +5 V
VSS: GND
MCLR: Reset externo


4.4 Arquitectura de Software / Firmware
El firmware se organiza en un lazo principal y una rutina de interrupción. En el lazo principal se realiza la lectura del ADC, el procesamiento de la medición del sensor y la actualización del valor mostrado en el display. La lectura analógica permite estimar el nivel del recipiente.
Timer0 se utiliza como base de tiempo para el refresco del display multiplexado. En cada interrupción de Timer0 se habilita un dígito del display y se cargan los segmentos correspondientes. Esta técnica permite visualizar varios dígitos usando menos pines del microcontrolador.
La interrupción externa RB0/INT se utiliza para el botón de emergencia. Cuando se detecta una pulsación válida, el sistema conmuta el estado de emergencia. En emergencia, la bomba se apaga, el LED conectado a RB4 se enciende y el display muestra “FAIL”. Al presionar nuevamente el botón, el sistema sale de emergencia y retorna al funcionamiento normal.

5. Especificaciones Eléctricas, Alimentación y Entorno
5.1 Parámetros de alimentación y consumo
Tensión de operación del sistema: 5 V 
Método de alimentación: fuente externa de 5 V 
Alimentación del PIC16F887: 5 V
Alimentación del sensor Sharp: 5 V
Alimentación de la bomba: 5 V 

5.2 PIC16F887
Herramientas de software
IDE: MPLAB X IDE
Simulación: Proteus
Lenguaje: Assembler para PIC16F887.
Ensamblador: MPASM.
Microcontrolador principal: PIC16F887.

Bits de configuración:
Oscilador: XT_OSC, utilizando cristal externo.
Watchdog Timer: OFF.
Master Clear: ON, utilizando pin externo MCLR.
Low Voltage Programming: OFF.
Brown-out Reset: BOR40V.
Write Protection: OFF.

Periféricos internos utilizados:
ADC: lectura de la señal analógica del sensor Sharp en AN8/RB2.
Timer0: refresco del display multiplexado.
Interrupción externa RB0/INT: botón de emergencia.
Puertos digitales:
PORTD para segmentos del display.
PORTA para habilitación de displays.
RB4 para LED de alarma.
RB7 para control de bomba mediante MOSFET.

Gestión de interrupciones:
El PIC16F887 cuenta con un único vector de interrupción, por lo que la prioridad se gestiona por software dentro de la rutina ISR. Dentro de la ISR se evalúan las banderas de interrupción para identificar el origen del evento.
La interrupción externa RB0/INT se considera crítica porque corresponde al botón de emergencia. Luego se atiende Timer0, utilizado para el refresco del display. Esta estrategia permite priorizar la seguridad del sistema frente a las tareas periódicas de visualización.

6. Proceso de Integración y Desarrollo
Etapa 1 – Validación inicial
Se comenzó verificando la configuración básica del PIC16F887, los puertos digitales y el funcionamiento del display de 7 segmentos. Primero se probó el encendido de segmentos individuales y luego el multiplexado utilizando Timer0. Esta etapa permitió validar la conexión entre PORTD y los segmentos, y entre PORTA y la habilitación de los dígitos.

Etapa 2 – Adquisición de señal analógica
Luego se incorporó el sensor Sharp conectado a AN8/RB2. Se configuró el ADC interno del microcontrolador y se realizaron pruebas variando la distancia entre el sensor y el objeto detectado. La lectura obtenida se utilizó para representar un nivel aproximado de llenado.

Etapa 3 – Integración lógica
Una vez verificados el display y la medición analógica, se implementó la lógica de interpretación del sensor. Según la lectura del ADC, el sistema actualiza el valor mostrado en el display. También se incorporó la lógica de estado normal y estado de falla.

Etapa 4 – Sistema completo
Finalmente se integró el botón de emergencia mediante RB0/INT, el LED de alarma en RB4 y el control de la bomba mediante RB7 y un MOSFET. En esta etapa se verificó que el sistema pudiera medir, visualizar, activar/desactivar el actuador y responder ante una emergencia.

8. Ensayos, Pruebas y Resultados
7.1 Pruebas funcionales realizadas
Prueba del display multiplexado
Se verificó el encendido de los cuatro dígitos del display de 7 segmentos. Se comprobó que Timer0 permite refrescar los dígitos de manera periódica, evitando parpadeos perceptibles.
Resultado esperado:
Visualización estable.
Segmentos correctos según el valor mostrado.
Habilitación individual de cada dígito.

Prueba del sensor Sharp
Se conectó el sensor Sharp a la entrada analógica AN8/RB2 del PIC16F887. Se midió la tensión de salida del sensor para distintas distancias y se comparó con el valor mostrado en el display.
Valores de calibración de referencia:
Distancia- Tensión aproximada- ADRESH aproximado- Interpretación
15 cm- 1,64 V- 84- Nivel alto / lleno
21 cm- 1,33 V- 68- Nivel medio
27 cm- 0,90 V- 46- Nivel bajo / vacío

Prueba del botón de emergencia
Se ensayó el pulsador conectado a RB0/INT. Al presionar el botón, el sistema debe entrar en estado de emergencia, mostrar “FAIL” en el display, apagar la bomba conectada a RB7 y encender el LED de alarma conectado a RB4. Mientras el pulsador esté presionado, la bomba está frenada y el led y display muestran la señal de alarma. CUando se suelta, el sistema vuelve a su estado normal. 

Prueba del actuador
Se verificó la salida digital RB7 que controla el MOSFET de la bomba. En estado normal, la bomba queda habilitada. En estado de emergencia, la salida se deshabilita para detener la bomba.
Además, se colocó un diodo de protección en paralelo con la bomba para evitar picos de tensión.

7.2 Evidencia fotográfica y gráficos
---

9. Resumen Final
El sistema desarrollado integra medición analógica, visualización de displays multiplexados, control digital de un actuador y gestión de emergencia mediante interrupción externa. El proyecto permite demostrar el uso coordinado de periféricos internos del PIC16F887, incluyendo ADC, Timer0, puertos digitales e interrupciones.
La maqueta funciona como una representación de un sistema automático de dosificación o llenado de fluidos. El sensor Sharp permite estimar el nivel del recipiente, el display informa el estado del sistema, la bomba actúa como elemento de dosificación y el botón de emergencia permite detener el proceso de forma segura.

