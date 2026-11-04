# Arduino Coffee Machine Prototype

A menu-driven coffee machine prototype built on an Arduino Uno (ATmega328P) and programmed in Flowcode v10. It was a solo university project, completed in about 2 weeks.

## What it does
The machine shows a menu on an LCD and lets the user choose a drink with push buttons:

- **Button 0: Maxi coffee.** The LCD shows "water heater turns on" and the temperature sensor is read continuously. When the temperature is at least 20 °C and below 30 °C, the buzzer beeps and the DC motor pumps water for 20 seconds, with a live countdown on the LCD. Then it shows "coffee ready".
- **Button 1: Espresso.** The same sequence, with the DC motor pumping water for 10 seconds.
- **Button 3: Emergency stop.** The LCD shows "emergency stop", the buzzer beeps 10 times, and the machine returns to the main menu. It works at every stage: while waiting for the right temperature and during the DC motor countdown.

The water heater and DC motor are represented by messages and a countdown on the LCD, not by physical parts.

## Hardware and components
- Arduino Uno (ATmega328P)
- LCD display (BL0169)
- EBLOCKS 2 switch board (BL0145): button 0 = Maxi, button 1 = Espresso, button 3 = Emergency
- Grove SHT31 temperature and humidity sensor (only temperature is used)
- Buzzer

## How it works
1. At start-up the LCD and temperature sensor are initialised and a welcome message is shown for 3 seconds.
2. The program waits for a button press and uses a Switch block to choose the Maxi, Espresso or Emergency path.
3. In the heating stage, a loop reads the temperature until it is at least 20 °C and below 30 °C, or until the emergency button is pressed.
4. A countdown on the LCD shows the remaining time while the DC motor pumps water. The emergency button is checked every second.
5. Delays slow the program down so messages can be read on the LCD.
6. After each drink or emergency stop, the program returns to the main menu.

## Demo on Arduino hardware
[paste your YouTube link here, or use: [Watch the demo](demo_arduino.mp4)]

## Simulation (Flowcode)
Menu screen:

![Menu](images/sim_1_menu.png)

Heating stage (reading the temperature):

![Heating](images/sim_2_maxi_heating.png)

DC motor pumping water, with countdown:

![Countdown](images/sim_3_motor_countdown.png)

Emergency stop:

![Emergency stop](images/sim_4_emergency.png)

## Flowchart
Overview of the whole program:

![Whole flowchart](images/flowchart_00_overview.png)

**Start-up and menu**

![Start-up and menu](images/flowchart_01_startup_menu.png)
![Waiting for a button press](images/flowchart_02_button_wait.png)

**Choosing a path (Switch block)**

![Switch block](images/flowchart_03_switch.png)

**Emergency stop path**

![Emergency stop](images/flowchart_04_emergency.png)

**Maxi coffee path**

![Maxi heating](images/flowchart_05_maxi_heating.png)
![Maxi DC motor countdown](images/flowchart_06_maxi_countdown.png)
![Loop end](images/flowchart_07_loop_end.png)

**Espresso path**

![Espresso heating](images/flowchart_08_espresso_heating.png)
![Espresso DC motor countdown](images/flowchart_09_espresso_countdown.png)
![Espresso end](images/flowchart_10_espresso_end.png)

## Challenges I solved
1. **Stuck in the temperature loop.** At first, if the temperature was never in the correct range, the program stayed in the temperature-reading loop with no way out. I fixed this by reading the switch board inside that loop and exiting if button 3 is pressed. The machine now goes straight to the emergency stop and back to the main menu.
2. **Emergency button ignored during the countdown.** While reviewing my flowchart, I noticed the countdown loops read the switch board but never stored the result in `switch_data`, so button 3 was not detected during the DC motor countdown. I changed the block to `switch_data = Switch_BL0145::ReadAll()` in both the Maxi and Espresso countdowns, and tested that the emergency stop now works at every stage.

## What I would improve
- Use a real heater and DC motor, with a higher target temperature for hot drinks.
- Add LEDs to show the machine status.

## Files
- `coffee_machine.fcfx`: Flowcode project file
- `coffee_machine.c`: generated C code. My program starts at `Use :User Project` (`int main()` is near the end). Everything above that is Flowcode's built-in library code.
- `images/`: simulation screenshots and flowchart sections
