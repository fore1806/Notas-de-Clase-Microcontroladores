
#   Apuntes Microcontroladores
Autor: Andrés Felipe Forero Salas 

GitHub nickname: [fore1806](https://github.com/fore1806) 

Microcontroladores

Universidad Nacional de Colombia Sede Bogotá

##  Videos Semana 3

### [PIC18F4550, Características y puertos de entrada salida] (https://www.youtube.com/watch?v=gQr_DS2BHHg)

####    Características

-   32 KB de memoria FLASH (máximo 16000 instrucciones de 1 palabra)
-   2 KB de memoria SRAM (Aprox 2000 variables máximo)
-   256 B de memoria EEPROM (Memoria no volátil)
-   Empaquetados de PDIP40 (40 pines para protoboard) y TQFP44 (44 pines en montaje superficial)
-   Fuente de reloj: Oscilador interno, externo y por ccristal con opción de PLL (Aumentar la frecuencia del oscilador externo o el cristal para disminuir perturbaciones o ruidos del oscilador)
-   34 pines I/O digitales y 13 pines análogos.
-   Puertos de comunicación: RS232, SPI, IIC, Paralelo y USB 2.0 Device
-   3 temporizadores de 16 bits y 1 de 8 bits (este último permite generar señales de PWM por dos canales independientes)

####    Oscilador

-   Oscilador interno. por defecto en 1 MHz, permite variaciones discretas entre 31 KHz, 125 KHz, 250 kHz, 500 kHz, 1 MHz, 2MHz, 4 MHz y 8 MHz.
-   PLL, permite obtener un oscilador hasta de 48 MHz, con una entrada de cristal de 4 MHz

####    Puertos I/O

-   Puertos de alimentación: VDD a 5 V; VSS a tierra. Se recomienda conectar estas entradas a los dos pares de pines, para mejor distribución de corriente en los módulos del microcontrolador y a su vez alargar la vida del dispositivo.

-   El PIC cuenta con dos pines para el oscilador (OSC1/CLKI; OSC2/CLKO/RA6), que solo deben emplearse para dicha función si se tiene un oscilador externo o cristal, en caso contrario se les puede dar otra funcionalidad.

-   Pin para uso del módulo USB (VUSB), se recomienda un condensador; en caso de no emplearlo dejarlo sin conexión.

-   Los demas pines son de I/O, la R dentoa pines digitales. Estos pines se organizan en 5 grupos

-   **Puerto A**, Agrupación de los 7 pines RA

-   **Puerto B**, Agrupación de los 8 pines RB

-   **Puerto C**, Agrupación de los 7 pines RC

-   **Puerto D**, Agrupación de los 8 pines RD

-   **Puerto E**, Agrupación de los 4 pines RE

-   Puertos completos, son aquellos que tienen tantos pines como el tamaño del PIC (8 bits), para este caso los ***puertos B y D son puertos completos***.

-   Puertos incompletos, son aquellos que tienen un número menor de pines al tamaño del PIC (8 bits), para este caso los ***puertos A, C y E son puertos incompletos***.

####    Configuración de puertos I/O

No todos los 34 pines I/O, comienzan con la función digital, esta debe ser configurada.

-   **PIN RB5:**     Por defecto tienen función LVP (Low Voltage Programming)

```asembler
    CONFIG LVP = OFF
```

-   **PIN RE3:**     Tiene funciones compartidas de MCLR (Master Clear), al liberarlo solo puede ser usado como ***entrada*** 

```asembler
    CONFIG MCLRE = OFF
```

-   **PIN RA6:**     Se utiliza en el módulo oscilador, para emplearlo, se debe utilizar el oscilador interno.
```asembler
    CONFIG FOSC = INTOSC_ECIO
```

-   **PIN RB0, RB1, RB2 y RB4:**     Tienen funciones del módulo ADC

```asembler
    CONFIG PBADEN = OFF
```

-   **PIN RC4 y RC5:**     Tienen funciones del módulo USB, al liberarlos solo pueden ser usados como ***entradas digitales***

```asembler
    bcf UCON, 3
```

-   **PIN RA0, RA1, RA2, RA3, RA4, RB0, RB1, RB2,  RB4, RE0, RE1 y RE2:**     Tienen funciones del módulo ADC, con la siguiente instrucción, la instrucción PBADEN seria una redundancia.

```asembler
    ;CONFIG PBADEN = OFF    ;redundancia
    movlw   .15
    movwf   ADCON1
```

De los 34 pines digitales, solo 31 pueden ser configurados como ***salidas digitales***; mientras que todos pueden ser ***entradas digitales***

La configuración de pines I/O se hace a través de los registros ***TRISx, LATx y PORTx*** , donde x se reemplaza por la letra del puerto a configurar.

##### Registro TRISx

Estos pines arrancan con un valor de 1, definiendo todos los pines como entrada; para definir alguna salida, debe definirse el pin como 0

```asembler
    bcf TRISB, 3    ;TRISB3 = 0, RB3 salida
    bsf TRISB, 0    ;TRISB0 = 1, RB0 entradada
    clrf TRISD      ;todo el puerto D como salida
    movlw b'00001111'   ;PIC omite el valor de RC3 que no existe ese pin
    movwf TRISC
```

##### Registro LATx

Estos pines arrancan indeterminados después de un reset, se encargan de establecer el valor lógico de los pines de salida digital, 0, establece un nivel bajo, 1, un nivel alto.

```asembler
    bcf LATB, 3    ;LATB3 = 0, RB3 salida en bajo
    bsf LATD, 1    ;LATD1 = 1, RB3 salida en alto
    setf LATD      ;todo el puerto D como salida alta
    movlw b'00001111'   ;PIC omite el valor de los pines inexistentes
    movwf LATE
```

##### Registro LATx

Se encargan de consultar el valor lógico de los pines de entrada digital, 0, representa un nivel bajo, 1, un nivel alto.

```asembler
    btfss PORTB,0   ;Si RB0 es 0
    btfsc PORTB,0   ;Si RB0 es 1
    ;leer todo el puerto
    movf PORTD,W
    movwf mivar ;mivar = PORTD
```

### [Descripción breve de diagramas de flujo] (https://www.youtube.com/watch?v=S23FwInZ7xA)

####    Características de diagramas de flujo

-   HYerramienta gráfica, para modelado y descripción de un algoritmo

-   Independientes del lenguaje de programación a utilizar, y de la plataforma de hardware en quye se implementará el algoritmo.

-   Existen diferentes bloques, que tienen una función determinada

-   Se tebe tener en cuenta aspectos como la presencia de un sistema operativo (computadores o microcontroladores) y su nivel de detalle.

####    Bloques Básicos

Bloque Inicio o Fin

-   Indica el inicio y final de un programa, se trata de bloques con forma de elipse, solo puede haber un bloque inicio, pero pueden existir diferentes bloques de fin dependiendo del camino.

![](https://github.com/fore1806/Notas-de-Clase-Microcontroladores/blob/master/images/inicio-fin.png)

Bloque Instrucción

-   Indica la ejecución de una instrucción, se recomienda describirlo con pseudocÓdigo

- Siempre se llega por arriba y se sale por abajo (Estética), son representados por rectangulos

![](images/instruccion.PNG)

Bloque Conjunto de instrucciones

-   Indica la ejecución de un conjunto de instrucciones

-   Se usan para representar una función común y reducir el tamaño general

![](images/InstructionSET.PNG)

Bloque Condicional

-   Indica toma de decisiones, sobre por donde seguir el camino, existe una condición lógica (verdadero o falso)

![](images/Condicional.PNG)

Bloque Iteración

Repetición de n veces de un set de instrucciones, representa una sentencia lógica de tipo ciclo en programación.

![](images/iteracion.PNG)

Se recomienda un nivel intermedio entre diagramas generales y detallados.

Diferenciar los recursos a utilizar, en computadores es común el bloque fin; sin embargo, para microcontroladores es poco frecuente.

Para describir funciones o subrutinas, se emplean flujos adicionales.