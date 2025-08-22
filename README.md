# Microproccsadores con base PIC/AVR pablinza@me.com
Programacion del Microprocesador en lenguaje C con el compilador oficial de Microchip __XC8__ <br />
Repositorio con las carpetas de ejercicios desarrolados durante el curso de Microprocesadores 2025 de la UEB <br />
El software __MPLABX__ disponible en la pagina del fabricante Microchip [ --> Click](https://ww1.microchip.com/downloads/aemDocuments/documents/DEV/ProductDocuments/SoftwareTools/MPLABX-v6.20-windows-installer.exe?authuser=0) <br />
El compilador __XC8__ puedes descargalo utilizando este enlace [ --> Click](https://ww1.microchip.com/downloads/aemDocuments/documents/DEV/ProductDocuments/SoftwareTools/xc8-v2.50-full-install-windows-x64-installer.exe?authuser=0) <br />
Para cargar el firmware al microcontrolador necesitaras un programador ICSP, como alternativa se utiliza el software __SimulIDE__ [ -->Click](https://simulide.com/p/) a efectos de verificar el funcionamiento <br />
Cada carpeta del proyecto MPLABX tiene el nombre precedido por el numero de actividad y en su estructura encontrara el programa principal con el nombre __main.c__ y librerias de uso local, una vez compilado el codigo se genera el firmware archivo __.hex__ en la carpeta dist/default/production. <br />

Programa base(PIC16F) __main.c__ para destellar un LED y que se utilizara en todas las practicas de este repositorio.
```c
//Ejemplo para Microcontrolador PIC16F687
#pragma config FOSC=INTRCIO, WDTE=OFF
#include <xc.h>
#define LED1pin PORTAbits.RA5
#define SW1pin PORTBbits.RB4

volatile __bit tickms;

void SetupMCU(void); //Configuracion del MCU
void TaskLED1(void); //Destello de led 

void main(void)
{
    SetupMCU();
    while(1)
    {
        if(tickms) //Valida cada 1ms
        {
            tickms = 0;
            TaskLED1();    
        }
        
    }
    return;
}
void SetupMCU(void)
{
    OSCCONbits.IRCF = 0b111; //Ajusta INTOSC a 8MHz
    /* CONFIGURACION PUERTOS */
    ANSEL = 0x00; //Desactiva pines ADC ANS0-7
    ANSELH = 0x00; //Desactiva pines ADC ANS8-13
    TRISAbits.TRISA5 = 0; //Salida LED1 pin RA5
    /* CONFIGURACION TIMER0 0.001s A FOSC=8MHz */
    OPTION_REGbits.T0CS = 0;//Modo Temporizador
    OPTION_REGbits.PSA = 0; //Con prescala
    OPTION_REGbits.PS = 0b011; //Prescala 1:16
    TMR0 = 131; //256-[(time*Fosc)/(pre*4)] time=0.001 seg
    INTCONbits.T0IE = 1; //Activa interrupcion del TMR0
    INTCONbits.GIE = 1;
}
void __interrupt() isr()
{
    if(INTCONbits.T0IF)
    {
        INTCONbits.T0IF = 0; //Limpia bandera
        TMR0 += 131;
        tickms = 1;
    }
}
void TaskLED1(void)
{
   static uint16_t cnt = 0;
    if(++cnt > 999) 
    {
        cnt = 0;
        LED1pin = 1;
    }
    if(cnt == 200) LED1pin = 0; 
}
```
<br />

## Lista de practicas desarrolladas con Microcontrolador

Autor: Pablo Zarate, puedes contactarme a pablinza@me.com / pablinzte@gmail.com.  <br />
Visita mi Blog  [Blog de Contenidos](https://pablinza.blogspot.com/). <br />
Visita mi Canal [Blog de Contenidos](http://www.youtube.com/@pablozarate7524). <br />
Santa Cruz - Bolivia 
<br clear="left"/>
