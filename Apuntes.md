
#   Apuntes Microcontroladores
Autor: Andrés Felipe Forero Salas 

GitHub nickname: [fore1806](https://github.com/fore1806) 

Microcontroladores

Universidad Nacional de Colombia Sede Bogotá

##  Videos Semana 1

### [Ensamblador](https://www.youtube.com/watch?v=rJJoBO52b0I&list=PLBE1L4ZEFvu4wgD_cVqE2S6kjkicAhwi2&index=1)

Ensamblador, lenguaje de máquina basado en ***mnemónicos***

```nasm
    incf var1   ;incrementar
    decf var2   ;decrementar
    comf LATD   ;Complemento a 1
    movf PORTB  ;Mover el valor de un registro
```

Ejemplo

```nasm
    include P18F4550.inc
    config FOSC = INTOSC_EC
    aux1 equ 0h
    Inicio
        clrf aux1   ;Setear en 0
    Menu
        incf aux1,f   ;suma 1 a aux1 y lo almacena en aux1
        goto Menu   ;Saltar a la etiqueta menu nuevamente
```
#### Secciones de un código

-   Inclusión de librerías y definición de símbolos
-   Directivas de configuración
-   Definición de variables
-   Instrucciones de ensamblador

##### Inclusión de librerías y definición de símbolos

```nasm
    include P18F4550.inc
    include milibreria.inc  ;Libreria propia
```
Se debe tener cuidado con el manejo de las direcciones de memoria de instrucciones a ña hora de crear librerías propias, para que no se sobrelapen los codigos de las librerías.

##### Directivas de configuración

-   Perro Guardian  (WDT)
-   Fuente de reloj (Interna, externa, reloj)
-   Protección de la memoria de instrucciones ante lectura
-   Asignación de pines I/O a algunos periféricos

##### Definición de variables

No existen tipos

2048 posiciones de memoria RAM desde ***0h*** hasta ***FFFh***

```nasm
    aux1 equ F45h   ;varName equ memoryPosition
```

Arreglos y vectores se logran con ***apuntadores***

##### Instrucciones de ensamblador

Estructura

```nasm
    Etiqueta
        Mnemónico   Opernados   ;Comentarios
```

```nasm
    Inicio
        clrf aux1   ;Setear en 0
        movff aux1,aux2 ;Copia de aux1 a aux2
    Menu
        nop     ;El microcontrolador no hace nada durante un ciclo de bus
        goto Menu   ;Saltar a la etiqueta menu nuevamente
```

### [Set de instrucciones](https://www.youtube.com/watch?v=5R3z-nYLlCg&list=PLBE1L4ZEFvu4wgD_cVqE2S6kjkicAhwi2&index=2)

Set RISC (Reduced Instruction Set Computing)

75 instrucciones más 8 instrucciones del set extendido (aplicaciones avanzadas)

Se pueden clasificar como

#### Según la operación

-   **Operaciones orientadas a byte.** Operaciones aritemticas, logicas o de movimiento, en las que se indica a la ALU que hacer con 8 bits de forma simultanea (addwf, incf, xorwf, decfsz)

-   **Operaciones orientadas a bit.** Operaciones sobre un unico bit de un registro o variable. (BSF, BCF, BTFSC)

-   **Operación de control.** Consultan o manipulan los registros de la CPU, expecto el W (nop, call)

-   **Operación de constantes.** Utilizan constantes para su ejecución (movlw, addlw, mullw, sublw)

```nasm
    movlw .5    ;. se usa para decimales
    addlw '@'   ;'simbolo'  se usa para asccii
    sublw '0x25'    ;x se usa para hexadecimal
    mullw b'11001010'   ;b'num' se usa para binario
    andlw o'167'    ;o'num' se usa para octal
    lfsr aux1   ; Se usa para apuntadores
```
### [Parte III](https://www.youtube.com/watch?v=fYE38mrrOjU&list=PLBE1L4ZEFvu4wgD_cVqE2S6kjkicAhwi2&index=3)

50 % del ciclo ON y 50 % OFF aproximadamente para el correcto funcionamiento de los flip flops internos del microcontrolador.



##  Videos Semana 3

### [PIC18F4550, Características y puertos de entrada salida](https://www.youtube.com/watch?v=gQr_DS2BHHg)

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

```nasm
    CONFIG LVP = OFF
```

-   **PIN RE3:**     Tiene funciones compartidas de MCLR (Master Clear), al liberarlo solo puede ser usado como ***entrada*** 

```nasm
    CONFIG MCLRE = OFF
```

-   **PIN RA6:**     Se utiliza en el módulo oscilador, para emplearlo, se debe utilizar el oscilador interno.

```nasm
    CONFIG FOSC = INTOSC_ECIO
```

-   **PIN RB0, RB1, RB2 y RB4:**     Tienen funciones del módulo ADC

```nasm
    CONFIG PBADEN = OFF
```

-   **PIN RC4 y RC5:**     Tienen funciones del módulo USB, al liberarlos solo pueden ser usados como ***entradas digitales***

```nasm
    bcf UCON, 3
```

-   **PIN RA0, RA1, RA2, RA3, RA4, RB0, RB1, RB2,  RB4, RE0, RE1 y RE2:**     Tienen funciones del módulo ADC, con la siguiente instrucción, la instrucción PBADEN seria una redundancia.

```nasm
    ;CONFIG PBADEN = OFF    ;redundancia
    movlw   .15
    movwf   ADCON1
```

De los 34 pines digitales, solo 31 pueden ser configurados como ***salidas digitales***; mientras que todos pueden ser ***entradas digitales***

La configuración de pines I/O se hace a través de los registros ***TRISx, LATx y PORTx*** , donde x se reemplaza por la letra del puerto a configurar.

##### Registro TRISx

Estos pines arrancan con un valor de 1, definiendo todos los pines como entrada; para definir alguna salida, debe definirse el pin como 0

```nasm
    bcf TRISB, 3    ;TRISB3 = 0, RB3 salida
    bsf TRISB, 0    ;TRISB0 = 1, RB0 entradada
    clrf TRISD      ;todo el puerto D como salida
    movlw b'00001111'   ;PIC omite el valor de RC3 que no existe ese pin
    movwf TRISC
```

##### Registro LATx

Estos pines arrancan indeterminados después de un reset, se encargan de establecer el valor lógico de los pines de salida digital, 0, establece un nivel bajo, 1, un nivel alto.

```nasm
    bcf LATB, 3    ;LATB3 = 0, RB3 salida en bajo
    bsf LATD, 1    ;LATD1 = 1, RB3 salida en alto
    setf LATD      ;todo el puerto D como salida alta
    movlw b'00001111'   ;PIC omite el valor de los pines inexistentes
    movwf LATE
```

##### Registro LATx

Se encargan de consultar el valor lógico de los pines de entrada digital, 0, representa un nivel bajo, 1, un nivel alto.

```nasm
    btfss PORTB,0   ;Si RB0 es 0
    btfsc PORTB,0   ;Si RB0 es 1
    ;leer todo el puerto
    movf PORTD,W
    movwf mivar ;mivar = PORTD
```

### [Descripción breve de diagramas de flujo](https://www.youtube.com/watch?v=S23FwInZ7xA)

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

![](https://github.com/fore1806/Notas-de-Clase-Microcontroladores/blob/master/images/instruccion.png)

Bloque Conjunto de instrucciones

-   Indica la ejecución de un conjunto de instrucciones

-   Se usan para representar una función común y reducir el tamaño general

![](https://github.com/fore1806/Notas-de-Clase-Microcontroladores/blob/master/images/InstructionSET.png)

Bloque Condicional

-   Indica toma de decisiones, sobre por donde seguir el camino, existe una condición lógica (verdadero o falso)

![](https://github.com/fore1806/Notas-de-Clase-Microcontroladores/blob/master/images/Condicional.png)

Bloque Iteración

Repetición de n veces de un set de instrucciones, representa una sentencia lógica de tipo ciclo en programación.

![](https://github.com/fore1806/Notas-de-Clase-Microcontroladores/blob/master/images/iteracion.png)

Se recomienda un nivel intermedio entre diagramas generales y detallados.

Diferenciar los recursos a utilizar, en computadores es común el bloque fin; sin embargo, para microcontroladores es poco frecuente.

Para describir funciones o subrutinas, se emplean flujos adicionales.


##  Videos Semana 4

### [Interrupciones en microcontroladores de 8 bits](https://www.youtube.com/watch?v=Fl5CL-wpzMk)

#### Interrupción: 

-   Rutinas de código que se ejecutan cuando un evento extraordianrio sucedió
-   Pueden ser periódica o aperiódica
-   La rutina de código que atienda la iterrupción se conoce como ***Interrupt Service Routine (ISR)***
-   Cuando ocurre la interrupción, se termina de ejecutar la instrucción actual, salva el contexto (valor del CP, W, STATUS) y luego va a ISR. 
-   En lenguajes como C, se puede parar en medio de una instrucción en ensamblador no
-   Al finalizar la ISR se recupera el contexto.
-   Usados para realizar tareas en paralelo
-   En **ensamblador** una ISR es una subrutina ubicada en una parte especial del código, requiere una configuración adicional. Estas ***nunca se llaman*** y deben culminar con alguna de las instrucciones.

```nasm
    retfie
    rti
```

-   En **C** las interrupciones son funciones, con una sintaxis especial para uqe el compilador pueda entenderlas como una ISR
-   **Vectores de Interrupción: ** Direcciones de memoria a las cuales se dirige el microcontrolador cuando ocurre el evento de interrupción

#### Eventos que generan una interrupción

-   Cambio en un pin de entrada 
-   sobreflujo de un temporizador
-   Culminación de una conversión análogo a digital (el módulo ADC se demora un tiempo realizando la conversión, es bueno saber cuando ha finalizado)
-   Recepción de un dato por comunicación serial
-   Error en un púerto de comunicaión (errores de marcado, o errores de colisiones de datos en protocolos sincronicos como el SPI)
-   Finalización de la transmisión de un dato

#### Configuración de interrupciones

-   Deben tener habilitación global y en casos habilitaciones por grupos.
-   Toda fuente de interrupción debe tener una habilitación individual conocida como máscara, ***Interrupt Enable (IE)***
-   Toda fuente de interrupción debe tener un indicador de ocurrencia conocido como bandera, ***Interrupt Flag (IF)***
-   Capa fuente de interrupción cuenta con un vector de interrupción (vectorizadas) depende del fabricante
-   No todas las fuentes de interrupción cuentan con un vector de interrupción (no vectorizadas), permite definir la prioridad, ***Interrupt Priority (IP)***
-   Una interrupción de ***alta prioridad*** puede interrumpir una de ***baja prioridad*** así, cuando se ejecute la de alta prioridad parará la de baja prioridad y cuando termine la de alta prioridad, se terminará la de baja para ahi si volver al código principal.
-   El PIC18F4550 cuenta con 21 fuentes de interrupción.

### [Interrupción externa 0 para el PIC18 con ejemplo en C](https://www.youtube.com/watch?v=06kbWiEcug8)

#### Configuración

-   Configurar el periférico o los periféricos
-   Seleccionar la prioridad (opcional)
-   Asegurarse que la bandera empiece en cero.
-   Habilitar individualmente la interruppción.
-   Habilitar el grupo de interrupciones si se requiere.
-   Habilitar globalmente las interrupciones

#### ISR

-   Ubicar el vector de interrupción correspondiente según la prioridad.
-   Implementar un algoritmo de "polling". Esto se requiere cuando existe más de una interrupción con la misma prioridad, consiste en preguntar cual de las interrupciones ocurrió.
-   Borrar la bandera de interrupción
-   Implementar el código de la ISR
-   Retornar de la interrupción según la sintaxis del lenguaje usado.

#### INT0

-   Solofunciona en el pin RB0 configurado como ***entrada digital***
-   Se puede configurar el flanco que lo dispara (subida o bajada)
-   Tiene su bit de habilitación **INT0IE**
-   Tiene su bit de indicación **INT0IF**
-   Siempre es de alta prioridad. Unica en el PIC18 que no se puede configurar su prioridad.

Código en ensamblador

```nasm
    ORG 0x0 ;Vector de Reset
        goto Inicio
    ORG 0x8 ;Vector de interrupciones de alta prioridad
        goto ISR    ;Etiqueta que representa la interrupción
    Inicio
        bsf INTCON2,6   ;Flanco de subida
        bcf INTCON,1    ;Borrar bandera
        bsf INTCON,4    ;Habilitación de la interrupción
        bsf INTCON,7    ;Habilitación global de interrupciones
    Menu
        ******************************************************
    ISR
        btfsc   INTCON,1    ;Algoritmo de polling, verifica cual interrupción se dio
        goto    ISR_INT0    ;Va a la ISR necesaria
        ******************************************************
    ISR_INT0
        bcf INTCON,1
        bsf LATD,0
        retfie
```

Código en C

```C
    void __interrupt() ISR(void);
    ***************************
    void main(void){
        INTEDG0 =1;
        INT0IF=0;
        INT0IE=1;
        GIE=1;
        **********************
    }
    void __interrupt() ISR(void){
        if(INT0IF==1){
            INT0IF=0;
            LATD0=1:
        }
        *************************
    }
```

### [Descripción y uso de retardos para microcontroladores](https://www.youtube.com/watch?v=8n-T8QwMPOQ)

#### Retardo:

-   Subrutinas dentro del lenguaje ensamblador, deben ser llamadas y retornar
-   La mas básica es la instrucción ***nop*** 
-   Se implementan a través de ciclos decrementales
-   El valor máximo dependera del microcontrolador, para el caso de uno de 8 bits, el ciclo más grande será 2 a la 8 menos 1, osea 255.
-   Si se requieren ciclos más grandes se deben utilizar ciclos anidados.

```nasm
    Retardo
        movlw   .81
        movwf   aux1
    AuxRetardo
        decfsz  aux1,f
        goto    AuxRetardo
        return
```