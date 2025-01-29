# Rotary-Encoder-with-STM32
Below is a simple code you can use to read rotary encoder values with an STM32 Microcontroller. The example below is for a Rotary Encoder that controls how fast an LED blinks. The full tutorial video is available on YouTube: https://youtu.be/9BJcLJLQJU8

    /* USER CODE BEGIN PV */
    int32_t rawCounter = 0;
    uint32_t counter = 0;
    uint32_t last_counter = 0;
    uint32_t blink_delay = 200; //Default blink delay
    /* USER CODE END PV */
    
    
    /* USER CODE BEGIN 2 */
    //Start Timer in Encoder mode
    HAL_TIM_Encoder_Start(&htim2, TIM_CHANNEL_ALL);
    /* USER CODE END 2 */
    
    
      while (1)
      {
        /* USER CODE END WHILE */
    
        /* USER CODE BEGIN 3 */
        // Read raw encoder count
    	  rawCounter = __HAL_TIM_GET_COUNTER(&htim2);  
    
    	  // Convert to a limited 0-20 range
    	  counter = (rawCounter / 4) % 21;  // Assuming 4 counts per step (adjust if needed)
    
    	  // Update blink delay only if the encoder value has changed
    	  if (counter != last_counter)
    	  {
    		  blink_delay = 10 + (counter % 100) * 10; // Map to 10ms - 1000ms
    		  last_counter = counter;
    	  }
    
    	  // Toggle LED and wait based on blink_delay
    	  HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_3);
    	  HAL_Delay(blink_delay);
      }
      /* USER CODE END 3 */

