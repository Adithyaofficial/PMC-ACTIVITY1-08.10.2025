# PMC-ACTIVITY1-08.10.2025

# Interfacing a 4x4 matrix keypad to display the output on LCD for keys 4,5,6.

# NAME : ADITYAH M S
# REG.NO : 212223220002

# PROGRAM :
```
#include "main.h"
#include "lcd.h"
#include "stdbool.h"

// Keypad column states
bool col1, col2, col3;

// Function prototypes
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
void key(void);

// LCD setup
Lcd_PortType ports[] = {GPIOA, GPIOA, GPIOA, GPIOA};
Lcd_PinType pins[] = {GPIO_PIN_3, GPIO_PIN_2, GPIO_PIN_1, GPIO_PIN_0};
Lcd_HandleTypeDef lcd;

int main(void)
{
  // HAL and clock init
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();

  // LCD initialize
  lcd = Lcd_create(ports, pins, GPIOB, GPIO_PIN_0, GPIOB, GPIO_PIN_1, LCD_4_BIT_MODE);

  Lcd_cursor(&lcd, 0, 0);
  Lcd_string(&lcd, "Keypad 4,5,6 Test");
  HAL_Delay(1000);
  Lcd_clear(&lcd);

  // Infinite loop
  while (1)
  {
    key();
  }
}

// Keypad scanning function (only keys 4, 5, 6)
void key(void)
{
  // Activate Row 2 (for keys 4,5,6)
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0, GPIO_PIN_SET);
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_1, GPIO_PIN_RESET);  // Row 2 active
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_2, GPIO_PIN_SET);
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_3, GPIO_PIN_SET);

  // Read columns
  col1 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_4);
  col2 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_5);
  col3 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_6);

  // Check for key press
  if(!col1)
  {
    Lcd_clear(&lcd);
    Lcd_cursor(&lcd, 0, 0);
    Lcd_string(&lcd, "Key 4 Pressed");
    HAL_Delay(500);
  }
  else if(!col2)
  {
    Lcd_clear(&lcd);
    Lcd_cursor(&lcd, 0, 0);
    Lcd_string(&lcd, "Key 5 Pressed");
    HAL_Delay(500);
  }
  else if(!col3)
  {
    Lcd_clear(&lcd);
    Lcd_cursor(&lcd, 0, 0);
    Lcd_string(&lcd, "Key 6 Pressed");
    HAL_Delay(500);
  }
}

void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  __HAL_RCC_PWR_CLK_ENABLE();
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE2);
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};

  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  // Keypad rows (PC0-PC3)
  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);

  // Keypad columns (PC4-PC7)
  GPIO_InitStruct.Pin = GPIO_PIN_4|GPIO_PIN_5|GPIO_PIN_6|GPIO_PIN_7;
  GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
  GPIO_InitStruct.Pull = GPIO_PULLUP;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);

  // LCD data pins (PA0-PA3)
  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

  // LCD control pins (PB0, PB1)
  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOB, &GPIO_InitStruct);
}

void Error_Handler(void)
{
  __disable_irq();
  while (1) {}
}
```
# OUTPUT :

<img width="1669" height="888" alt="image" src="https://github.com/user-attachments/assets/912e560c-409c-4260-a58b-97990d2b868d" />
<img width="1755" height="844" alt="image" src="https://github.com/user-attachments/assets/e40bcc60-a886-4167-8e47-89741b3734d5" />
<img width="1753" height="852" alt="Screenshot 2025-10-08 212411" src="https://github.com/user-attachments/assets/294e9132-5c10-40a2-b74b-55024ed6e127" />
<img width="1729" height="850" alt="Screenshot 2025-10-08 212426" src="https://github.com/user-attachments/assets/d52067f2-7ee9-49ad-a3c5-3d5c3bb1408b" />
<img width="1757" height="848" alt="Screenshot 2025-10-08 212507" src="https://github.com/user-attachments/assets/7306bf92-1fe8-4676-a7e5-cb994935d9e2" />
