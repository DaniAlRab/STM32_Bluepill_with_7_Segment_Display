# STM32_Bluepill_with_7_Segment_Display 🚧
![Under Construction](https://img.shields.io/badge/status-under--construction-yellow)

This little project shows how to use an STM32 with a 7 Segment Display without using any specific library for the display

1º - Identify your 7-segment display type.
Common Anode
Common Cathode

2º - Choose the pins that will be used to connect to the display segments.


ex:
These pins have to be configured as outputs by editing the .ioc file in your project.

{GPIOA, GPIO_PIN_10}, // A

{GPIOB, GPIO_PIN_6}, // B

{GPIOB, GPIO_PIN_4}, // C

{GPIOA, GPIO_PIN_11}, // D

{GPIOB, GPIO_PIN_5}, // E

{GPIOA, GPIO_PIN_9}, // F

{GPIOA, GPIO_PIN_8}, // G

{GPIOA, GPIO_PIN_12} // DP

3º - Create a digit map based on your pin choice.


´´´
//DIGIT MAP FOR ANODE COMMON 7 SEGMENTS DISPLAY

const uint8_t digitMap[] = {
  0b11000000, // 0
  0b11111001, // 1
  0b10100100, // 2
  0b10110000, // 3
  0b10011001, // 4
  0b10010010, // 5
  0b10000010, // 6
  0b11111000, // 7
  0b10000000, // 8
  0b10010000, // 9
  0b10001000, // A
  0b10000011, // B
  0b11000110, // C
  0b10100001, // D
  0b10000110, // E
  0b10001110  // F
};

´´´

Explaining digit MAP

Ex: DIGIT 0

0 b 1  1  0  0  0  0  0  0   // 0
    DP G  F  E  D  C  B  A   // SEGMENTS 


This digit Map makes it easier to change digits without setting them segment by segment.
This one works with the function SetDigit, which receives just one uint_8 digit and "translates" it to a 7-segment display


void SetDigit(uint8_t digit)
{
    if (digit > 17) return;

    uint8_t pattern = digitMap[digit];

    for (int i = 0; i < 8; i++) // A to DP
    {
        GPIO_PinState state = ((pattern >> i) & 0x01) ? GPIO_PIN_SET : GPIO_PIN_RESET;

        HAL_GPIO_WritePin(seg[i].port, seg[i].pin, state);
    }
}




    







