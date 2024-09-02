# Daisy Seed OLED 1306 I2C/ 2 Wire (gnd, vcc, scl, sdc)
Example of using OLED with Electrosmith Daisy Seed music microcontroller.

![daisy_seed_oled_gnd_vcc_scl_sda](https://github.com/user-attachments/assets/9ce37531-b499-45bb-9448-2c939d4988fe)

### Wiring
### Fork / Clone ( https://github.com/electro-smith/DaisyExamples/tree/master )
### Copy OLED example
Use the helper script to copy the OLED example into a new folder.

```shell
./helper.py copy seed/IC2_OLED --source seed/OLED
```
Change directories into the new folder:
```shell
cd ./seed/IC2OLED
```
### Config the copied OLED code
```cpp
#include <stdio.h>
#include <string.h>
#include "daisy_seed.h"
#include "dev/oled_ssd130x.h"

using namespace daisy;

/** Typedef the OledDisplay to make syntax cleaner below 
 *  This is a I2C Transport controlling an 128x64 sized SSDD1306
 * 
 *  There are several other premade test 
*/
using MyOledDisplay = OledDisplay<SSD130xI2c128x32Driver>;

DaisySeed     hw;
MyOledDisplay display;

int main(void)
{
    uint8_t message_idx;
    hw.Configure();
    hw.Init();

    /** Configure the Display */
    MyOledDisplay::Config display_config;

    display_config.driver_config.transport_config.i2c_address = 0x3C;
	display_config.driver_config.transport_config.i2c_config.periph = I2CHandle::Config::Peripheral::I2C_1;
	display_config.driver_config.transport_config.i2c_config.speed  = I2CHandle::Config::Speed::I2C_100KHZ;
	display_config.driver_config.transport_config.i2c_config.mode = I2CHandle::Config::Mode::I2C_MASTER;
 	display_config.driver_config.transport_config.i2c_config.pin_config.scl = {DSY_GPIOB, 8};
	display_config.driver_config.transport_config.i2c_config.pin_config.sda = {DSY_GPIOB, 9};

	display.Init(display_config);
    
     
    /** And Initialize */
    display.Init(display_config);

    message_idx = 0;
    char strbuff[128];
    while(1)
    {
        System::Delay(500);
        switch(message_idx)
        {
            case 0: sprintf(strbuff, "Testing. . ."); break;
            case 1: sprintf(strbuff, "Daisy. . ."); break;
            case 2: sprintf(strbuff, "1. . ."); break;
            case 3: sprintf(strbuff, "2. . ."); break;
            case 4: sprintf(strbuff, "3. . ."); break;
            default: break;
        }
        message_idx = (message_idx + 1) % 5;
        display.Fill(true);
        display.SetCursor(0, 0);
        display.WriteString(strbuff, Font_11x18, false);
        display.Update();
    }
}
```

### Compile
```shell
make .
```
### Reset seed
Press both white buttons.
### Upload
```shell
make program-dfu
```



