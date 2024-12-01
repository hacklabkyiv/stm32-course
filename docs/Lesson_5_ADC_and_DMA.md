# Multichannel ADC Reading and DMA
Here is quick notes about capturing ADC data in DMA mode. We will use "Black Pill" development board with STM32F401CCU6 MCU onboard.
[Link to ST document about ADC](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://www.st.com/resource/en/application_note/an3116-stm32s-adc-modes-and-their-applications-stmicroelectronics.pdf&ved=2ahUKEwji5-PWqYaKAxUVQ_EDHa7_DoAQFnoECBkQAQ&usg=AOvVaw03qbfBZEKEd9gASszJ2g-L)

1. Start new project in STM32CubeIDE as usual. Choose:
  - System Core -> RCC: High Speed Clock -> Crystal/Ceramic Resonator
                -> Debug -> Serial Wire
2. Enable HSE in Clock settings:

![image  align=left](https://github.com/user-attachments/assets/7899ecd4-9b34-4181-b04b-5c339b3d7d1e)

3. In "ADC" check "IN0" and "IN1" checkboxes
4. In "DMA Setings" inside ADC section:

![image align=left](https://github.com/user-attachments/assets/cb650c72-9266-427c-8883-5cb1537521ee)

4. In Prameter Settings:

![image align=left](https://github.com/user-attachments/assets/deed2cc1-1461-478a-b07b-c0a9df8381db)

6. Save and Generate code
7. Go to main.c and write this code in "USER CODE BEGIN 0" section.
```

uint32_t adc_data[2] = {0};
void HAL_ADC_ConvCpltCallback(ADC_HandleTypeDef *hadc)
{
  // Do callback logic here
  int i = 0;
}

int main(void)
{

  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();
  MX_DMA_Init();
  MX_ADC1_Init();
  MX_TIM1_Init();
  // ADD THIS LINE
  // Length of data is 2, since "Data width" in DMA section is "word"
  // In 32-bit architecture, word is 32 bit and our adc_data
  // constists of 2 words
  HAL_ADC_Start_DMA(&hadc1, adc_data, 2);
  while (1)
  {
  }
}
```
