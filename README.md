```
Name: ABINAYA S
Reg No: 212222230002
```
# EXPERIMENT 05 INTERFACING A 4X4 MATRIX KEYPAD AND DISPLAY THE OUTPUT ON LCD

## Aim: 
To Interface a 4X4 matrix keypad and show the output on 16X2 LCD display to ARM controller , and simulate it in Proteus
## Components required: 
STM32 CUBE IDE, Proteus 8 simulator .
## Theory:

<img src=https://github.com/vasanthkumarch/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/36288975/2a4a795e-1674-4329-ae07-3f5e8d5073e2 width=450 height=450>

4×4 Keypad Module Pin Diagram
 
4x4 Keypad module Pin Diagram
4×4 Keypad module Pin Diagram
Pin Number	Pin Name	Description
1	R1	Taken out from 1st  ROW
2	R2	Taken out from 2nd  ROW
3	R3	Taken out from 3rd  ROW
4	R4	Taken out from 4th  ROW
5	C1	Taken out from 1st  COLUMN
6	C2	Taken out from 2nd COLUMN
7	C3	Taken out from 3rd  COLUMN
8	C4	Taken out from 4th  COLUMN
4×4 Matrix Keypad Module Hardware Overview
These Keypad modules are made of thin, flexible membrane material. The 4 x4 keypad module consists of 16 keys, these Keys are organized in a matrix of rows and columns. All these switches are connected to each other with a conductive trace. Normally there is no connection between rows and columns. When we will press a key, then a row and a column make contact.

 
## Procedure:
Select a new STM32 Project.

Select GPIO Ports
```

  PC0 , PC1 , PC2 , PC3 , PA0 , PA1 , PA2 , PA3 , PB0 , PB1  -> Output

  PC4 , PC5 , PC7 , PC8  -> Input
```
Configure the Input Ports at Pull up Mode followed by generating the code.

Build Debug and Create 'hex.file'

Open a new Proteus Project.

Select STM32F401RB, LCD 16*2 and Keypad.

Connect PA0 to D7 , PA1 to D6 , PA2 to D5 , PA3 to D4 , PB0 to RS , PB1 to E , PC0 to r1 , PC1 to r2 , PC2 to r3 , PC3 to r4 , PC4 to c1 , PC5 to c2 , PC6 to c3 and PC7 to c4.

Check the execution of the output using Run Option.
 


## STM 32 CUBE PROGRAM :
```
#include "main.h"
#include "lcd.h"
#include <stdbool.h>
bool col1,col2,col3,col4;
void key();

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
void key()
{

	Lcd_PortType ports[] = { GPIOA, GPIOA, GPIOA, GPIOA };
	Lcd_PinType pins[] = {GPIO_PIN_3, GPIO_PIN_2, GPIO_PIN_1, GPIO_PIN_0};
	Lcd_HandleTypeDef lcd;
	lcd = Lcd_create(ports, pins, GPIOB, GPIO_PIN_0, GPIOB, GPIO_PIN_1, LCD_4_BIT_MODE);

	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0, GPIO_PIN_RESET);
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_1, GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2, GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_3, GPIO_PIN_SET);

	col1 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_4);
	col2 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_5);
	col3 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_6);
	col4 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_7);

	if(!col1)
	{
		Lcd_cursor(&lcd, 0,1);
		Lcd_string(&lcd, "Key 7\n");
		HAL_Delay(500);
	}
	else if(!col2)
	{
		Lcd_cursor(&lcd, 0,1);
		Lcd_string(&lcd, "Key 8\n");
		HAL_Delay(500);
	}
	else if(!col3)
	{
		Lcd_cursor(&lcd, 0,1);
		Lcd_string(&lcd, "Key 9\n");
		HAL_Delay(500);
	}
	else if(!col4)
	{
		Lcd_cursor(&lcd, 0,1);
		Lcd_string(&lcd, "Key %\n");
		HAL_Delay(500);
	}
}

int main(void)
{ 
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();
  while (1)
  {
 	  key();
   }
 }
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};


  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, GPIO_PIN_RESET);

 
  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, GPIO_PIN_RESET);

 
  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0|GPIO_PIN_1, GPIO_PIN_RESET);

  
  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);

 
  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

  
  GPIO_InitStruct.Pin = GPIO_PIN_4|GPIO_PIN_5|GPIO_PIN_6|GPIO_PIN_7;
  GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
  GPIO_InitStruct.Pull = GPIO_PULLUP;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);


  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOB, &GPIO_InitStruct);

}
```


## Output screen shots of proteus  :
 ### BUTTON OFF
 <img src="https://github.com/abinayasangeetha/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/119393675/2fd9d80c-d4f6-428b-a3f6-2337d3e08263 " width=450 height=450>


 ### BUTTON ON
 <img src="https://github.com/abinayasangeetha/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/119393675/ab6894ab-233c-4355-9fd6-e0c8de13a12c " width=450 height=450>

 
 ## CIRCUIT DIAGRAM (EXPORT THE GRAPHICS TO PDF AND ADD THE SCREEN SHOT HERE): 
 <img src="https://github.com/abinayasangeetha/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/119393675/ba89ec9c-3ad8-4315-9caa-54ff72d065a5 " width=450 height=450>

 
## Result :
Interfacing a 4x4 keypad with ARM microcontroller are simulated in proteus and the results are verified.
