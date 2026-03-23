# pcb-board-with-microcontroler-
Microcontroller-based DC motor fan with custom PCB, designed for embedded systems control.

## Features
- Microcontroller-based control of DC motor  
- PWM-based speed regulation   
- Custom PCB design  
- Embedded firmware for motor control  

## Technologies Used
- Microcontroller ATmega328P 
- Embedded C++  
- PCB design software KiCad 
- DC motor driver   

## Project Description
The goal of this project was to design and build a system capable of controlling the speed of a DC motor using a microcontroller.

The project includes:
- Schematic design  
- PCB layout  
- Firmware implementation  
- Integration of hardware and software  

## My Contribution
- Designed the PCB  
- Developed the embedded firmware  
- Assembled and tested the system

## Code
</> Markdown
```c
#include <avr/io.h>

// -----------------------------
// Inicjalizacja ADC (PC0)
// -----------------------------
void ADC_init(void)
{
    ADMUX = (1 << REFS0); 
    // REFS0 = 1 → napięcie odniesienia AVcc
    // MUX = 0000 → ADC0 (PC0)

    ADCSRA = (1 << ADEN)  |  // włącz ADC
             (1 << ADPS2) | (1 << ADPS1) | (1 << ADPS0);
    // preskaler 128
}

// -----------------------------
// Odczyt ADC
// -----------------------------
uint16_t ADC_read(void)
{
    ADCSRA |= (1 << ADSC);          // start konwersji
    while (ADCSRA & (1 << ADSC));   // czekaj na koniec
    return ADC;
}

// -----------------------------
// Inicjalizacja PWM (Timer0, PD6)
// -----------------------------
void PWM_init(void)
{
    DDRD |= (1 << PD6); // PD6 (OC0A) jako wyjście

    TCCR0A = (1 << WGM00) | (1 << WGM01) | // Fast PWM
             (1 << COM0A1);               // PWM na OC0A

    TCCR0B = (1 << CS01); // preskaler 8

    OCR0A = 0; // startowo 0% wypełnienia
}

int main(void)
{
    // -----------------------------
    // Konfiguracja pinów
    // -----------------------------
    DDRB |= (1 << PB1);     // dioda
    PORTB |= (1 << PB1);    // dioda świeci

    DDRD |= (1 << PD7);     // PD7 kierunek silnika

    // Kierunek obrotu
    PORTD |= (1 << PD7);    // ustaw jeden kierunek

    ADC_init();
    PWM_init();

    // -----------------------------
    // Pętla główna
    // -----------------------------
    while (1)
    {
        uint16_t adc_value = ADC_read(); // 0–1023
        OCR0A = adc_value / 4;           // 0–255 (PWM)
    }
}
...
```


## Images / Schematics
<img width="1332" height="620" alt="image" src="https://github.com/user-attachments/assets/a561d302-f2f0-40ba-b067-b0ec76d47d5a" />


<img width="1278" height="778" alt="image" src="https://github.com/user-attachments/assets/73128ecc-a2bd-4b2e-a4e4-137b770789b5" />


## Contact
If you have any questions, feel free to contact me via GitHub.
