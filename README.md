#include "mcc_generated_files/mcc.h"

volatile uint8_t appui = 0;

typedef struct {
    uint8_t f1;
    uint8_t f2;

} sounds;

void IT_ExtS1(void);
void IT_ExtS2(void);
void frequence(sounds buzer);

/*
                         Main application
 */
void main(void) {
    // initialize the device
    SYSTEM_Initialize();
    sounds S1 = {.f1 = 599, .f2 = 0};
    sounds S2 = {.f1 = 0, .f2 = 299};
    sounds S3 = {.f1 = 0, .f2 = 0};

    // When using interrupts, you need to set the Global and Peripheral Interrupt Enable bits
    // Use the following macros to:
    IOCBF1_SetInterruptHandler(IT_ExtS1);
    IOCBF2_SetInterruptHandler(IT_ExtS2);

    // Enable the Global Interrupts
    INTERRUPT_GlobalInterruptEnable();

    // Enable the Peripheral Interrupts
    //INTERRUPT_PeripheralInterruptEnable();

    // Disable the Global Interrupts
    //INTERRUPT_GlobalInterruptDisable();

    // Disable the Peripheral Interrupts
    //INTERRUPT_PeripheralInterruptDisable();

    while (1) {
        // Add your application code
        switch (appui) {
            case 1:
                frequence(S1);
                D3_SetHigh();
                __delay_ms(1000);
                frequence(S3);
                D3_SetLow();
                appui = 0;
                break;
            case 2:
                frequence(S2);
                D3_SetHigh();
                __delay_ms(1000);
                frequence(S3);
                D3_SetLow();
                appui = 0;
                break;
            default:
                frequence(S3);
                break;

        }
    }
}

void frequence(sounds buzer) {
    PWM1_LoadDutyValue(buzer.f1 >> 2);
    PWM6_LoadDutyValue(buzer.f2 >> 2);
}

void IT_ExtS1(void) {
    appui = 1;
}

void IT_ExtS2(void) {
    appui = 2;
}




/**
 End of File
 */
