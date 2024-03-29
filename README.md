# LED_controller
This node controls a WS2812B LED strip depending on the state of the robot in accordance with the URC 2022 rule book for the autonomous navigation mission.


| State              | Light          |
|--------------------|----------------|
| Navigating to goal | Solid red      |
| Goal reached       | Flashing green |
| Teleoperation      | Solid blue     |


## Subscribed topics
- goal_manager/state (std_msgs/Int8): State of the goal_manager (0: Nothing, 1: Autonomous navigation in progress, 2: Goal reached, 3: teleoperation)

# Setup

### Dependencies
1. Python 3.7 or higher is needed. If it is not already installed you can do so with:
    ```
    sudo apt install python3.8
    sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1
    ```
2. To make python3 compatible with ROS Melodic:
    ```
    sudo apt-get install python3-catkin-pkg-modules
    sudo apt-get install python3-rospkg-modules
    ```
3. Install the *CircuitPython NeoPixel SPI* library
    ```
    pip3 install adafruit-circuitpython-neopixel-spi
    ```

### Enable SPI
1. `sudo /opt/nvidia/jetson-io/jetson-io.py`
2. Select *Configure 40-pin expansion header* then select *Configure header pins manually*.
3. Select *spi1 (19, 21, 23, 24, 26)*
4. Select Save pin changes and then Save and reboot to reconfigure pins.
5. After the Nano boots up again, verify you have the I2C devices with the command `ls /dev/i2c* /dev/spi*`. If you don't see the SPI devices, try using this command: `sudo modprobe spidev`. You will need to do this command at every boot, or make it run at automatically on startup.
6. Set user permissions
    ```bash
    sudo groupadd -f -r gpio
    sudo usermod -a -G gpio <user>
    ```
7. Copy rules
    ```bash
    sudo cp 99-gpio.rules /etc/udev/rules.d
    ```
8. Reboot

### Wiring
Wire the NeoPixel's `DIN` pin to the Nano's `SPI_1_MOSI` pin (19) and the ground wire in a GND pin (20)

TODO: document the full wiring including the level-shifter setup