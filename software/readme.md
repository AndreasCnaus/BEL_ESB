# BME280 HAL-Driver for STM32WB based custom board

This library contains the driver code for the Bosch-BME280 sensor tailored for the STM32WB based Bluetooth breakout board (see also hardware section).
The initialization of the peripherals, the timer as well as the sensor configuration and the reading of the sensor's compensation data ​​occurs once before entering the endless while loop in the main routine.
The reading of the sensor data is controlled by the timer interrupt. To keep the interruption lightweight, only one flag (*tim16_flag*) is set there, which is queried and reset in the main loop.
The raw sensor data is read via I2C interface using the STM32-HAL library. These raw data are compensated after readingd according to the BME280 data sheet (see link below) and converted into a form understandable for the nRF application. 
Finally, the BLE characteristics are updated with the new data. 

The BLE middleware for the STM32WB microcontroller was configured as a server profile that includes the Environmental Sensing Service with three characteristics: temperature, pressure and humidity (see also Environmental sensing profile in the links below).
The software presented here is simply intended to proof the concept. For this reason, quite simple means such as flag checking in the main loop were used. For more extensive applications, a sequencer should be used instead of simple flags (see also  AN5289 specially the part 4.5 in the links below).

The following hardware configuration was made in CubeIDE for this project. The blue LED at pin *PA10* is used as an error indicator and makes debugging easier.

![pinout_view](https://github.com/user-attachments/assets/07986607-2466-4007-8c5b-5ebdd6c286d6)

# Links
- BME280 Datasheet: https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme280-ds002.pdf
- Environmental sensing profile: https://www.bluetooth.com/specifications/specs/environmental-sensing-profile-1-0/
- AN5289: https://www.st.com/resource/en/application_note/dm00598033-building-wireless-applications-with-stm32wb-series-microcontrollers-stmicroelectronics.pdf
