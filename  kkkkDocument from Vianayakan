#include "main.h"

ADC_HandleTypeDef hadc1;

#define BUZZER_PIN GPIO_PIN_0
#define BUZZER_PORT GPIOB

#define LED_PIN GPIO_PIN_1
#define LED_PORT GPIOB

#define GAS_THRESHOLD 2000   // Adjust after calibration

uint32_t gasValue;

uint32_t Read_GasSensor(void)
{
    HAL_ADC_Start(&hadc1);
    HAL_ADC_PollForConversion(&hadc1, 100);
    return HAL_ADC_GetValue(&hadc1);
}

int main(void)
{
    HAL_Init();
    SystemClock_Config();

    MX_GPIO_Init();
    MX_ADC1_Init();

    while (1)
    {
        gasValue = Read_GasSensor();

        if(gasValue > GAS_THRESHOLD)
        {
            // Gas Leakage Detected

            HAL_GPIO_WritePin(BUZZER_PORT,
                              BUZZER_PIN,
                              GPIO_PIN_SET);

            HAL_GPIO_WritePin(LED_PORT,
                              LED_PIN,
                              GPIO_PIN_SET);

            // Send IoT Alert Function
            Send_GasAlert();
        }
        else
        {
            HAL_GPIO_WritePin(BUZZER_PORT,
                              BUZZER_PIN,
                              GPIO_PIN_RESET);

            HAL_GPIO_WritePin(LED_PORT,
                              LED_PIN,
                              GPIO_PIN_RESET);
        }

        HAL_Delay(500);
    }
}