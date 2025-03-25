# arduino-projects-code

<a href='https://docs.arduino.cc/'>Arduino Docs</a>

Menu: 

* [Basic code editing](#basic-code-editing)
* [Blinking Led](#blinking-led)
  - [using delay()](#using-delay)
  - [using millis()](#using-millis)
* [Pins](#pins)
  - [Digital Pins](#digital-pins)
  - [Analog Input Pins](#analog-input-pins)
  - [Power and Ground Pins](#power-and-ground-pins)
  - [Reset Pin](#reset-pin)
* [Question 1](#question-1)
* [Traffic Lights](#traffic-lights)
* [Pushbutton](#pushbutton)
* [Question 2](#question-2)
* [Question 3](#question-3)
* [Question 4](#question-4)
  - [Using Two Pushbuttons](#using-two-pushbuttons)
* [Serial Monitor](#serial-monitor)
  - [Transmitting serial data](#transmitting-serial-data)
  - [Receiving serial data](#receiving-serial-data)
* [Question 5](#question-5)
* [Traffic Lights with pedestrian](#traffic-ights-with-pedestrian)

 ## Basic Code Editing

<details>
  <summary>Source</summary>
	
 <a href="https://docs.arduino.cc/built-in-examples/basics/Blink/#code">Arduino Docs</a>  
  </details>

 <code>void setup() {
  Serial.begin(9600); // Start serial communication
  int num = 42;       // Local variable
  pinMode(LED_BUILTIN, OUTPUT);
  
  Serial.print("Size of num: ");
  Serial.println(sizeof(num)); // Print size of int (typically 2 or 4 bytes)
}
void loop() {
  int ledState = HIGH; // Local variable
  digitalWrite(LED_BUILTIN, ledState);
  delay(1000);
  ledState = LOW;
  digitalWrite(LED_BUILTIN, ledState);
  delay(1000);
}
</code>

## Blinking LED

#### using delay()
**Code**
<details>
  <summary>Source</summary>
	
 <a href="https://docs.arduino.cc/built-in-examples/basics/Blink/#code">Arduino Docs</a>  
  </details>

<code>int red = 5;
int time = 1000 //time delay;
void setup() {
	// initialize colored led red connected to digital pin 5 as an output.
	pinMode(red, OUTPUT);
}
// the loop function runs over and over again forever
void loop() {
	digitalWrite(red, HIGH);  // turn the LED on (HIGH is the voltage level)
	delay(time);                      // wait for certain amount of time 
	digitalWrite(red, LOW);   // turn the LED off by making the voltage LOW
	delay(time);                    
}
</code>

<details>
  <summary>Source</summary>
	
 <a href="https://docs.arduino.cc/built-in-examples/digital/BlinkWithoutDelay/">Arduino Docs</a>  
  </details>

#### using millis()

<code>// constants won't change. Used here to set a pin number
const int ledPin = 5;  // the number of the LED pin 5
// Variables will change:
int ledState = LOW;  // ledState used to set the LED
// use "unsigned long" for variables that hold time
// The value will quickly become too large for an int to store
unsigned long previousMillis = 0;  // will store last time LED was updated
// constants won't change:
const long interval = 1000;  // interval at which to blink (milliseconds)
void setup() {
	// set the digital pin as output:
	pinMode(ledPin, OUTPUT);
}
void loop() {
	// check to see if it's time to blink the LED; that is, if the difference
	// between the current time and last time you blinked the LED is bigger than
	// the interval at which you want to blink the LED.
	unsigned long currentMillis = millis();
	if (currentMillis - previousMillis >= interval) {
		// save the last time you blinked the LED
		previousMillis = currentMillis;
		// if the LED is off turn it on and vice-versa:
		if (ledState == LOW) {
			ledState = HIGH;
		} else {
			ledState = LOW;
		}
		// set the LED with the ledState of the variable:
		digitalWrite(ledPin, ledState);
	}
}
</code>

<table>
	<tr><th>When OUTPUT is HIGH(led is blinking red)</th>
	<th>When OUTPUT is LOW(led is not blinking)</th></tr>
	<tr><td><img width=400 height=auto src='https://github.com/user-attachments/assets/3e117b31-fa0b-4fd1-88d2-8791b6f98796'></td>
	<td><img width=400 height=auto src='https://github.com/user-attachments/assets/1a517274-f4a0-4a09-81f3-9c910e354c25'>
</td></tr>
</table>

## Pins

#### Digital Pins

<details>
  <summary>Source</summary>
	
 <a href="https://docs.arduino.cc/learn/microcontrollers/digital-pins/">Arduino Docs</a>  
  </details>
The pins on the Arduino can be configured as either inputs or outputs. 

**Pin as INPUT**

Input pins are highly sensitive and require very little current to change state, similar to having a 100 MΩ resistor in series. This makes them useful for capacitive touch sensors, LED-based light detection, or analog sensing using techniques like RCTime.

However, if an input pin is left unconnected (<code>pinMode(pin, INPUT);</code>), it can pick up electrical noise or be influenced by nearby pins, leading to random state changes.

**Pin as OUTPUT**

OUTPUT pins are in a low-impedance state, meaning they can provide significant current (up to 40 mA) to other circuits. This is enough to power LEDs (with a resistor) and some sensors, but not high-power devices like relays, solenoids, or motors.

Short circuits or excessive current draw can damage the pin’s output transistors or even the entire Atmega chip. To prevent this, it's recommended to use 470Ω or 1kΩ resistors when connecting OUTPUT pins to other devices unless maximum current is necessary.

#### Analog Input Pins

The Atmega328 has several pins that can function as analog inputs. These pins can measure continuous voltage levels, allowing you to interface with analog sensors like
temperature sensors, light sensors, potentiometers, etc. The microcontroller's built-in analog-to- digital converter (ADC) converts the analog voltage into a digital value that can be processed by the microcontroller.

The analog pins can be used identically to the digital pins, using the aliases A0 (for analog input 0), A1, etc. For example, the code would look like this to set analog pin 0 to an output, and to set it HIGH:
<code>pinMode(A0, OUTPUT);
digitalWrite(A0, HIGH);</code>

#### Power and Ground Pins

Power and Ground Pins: Essential for powering the microcontroller and providing a reference voltage. VCC (power supply voltage) and GND (ground) pins are critical for proper operation.

#### Reset Pin

Reset Pin: This pin is used to reset the microcontroller. Applying a reset pulse to this pin initializes the microcontroller's program execution from the beginning.

## Question 1

> The LED peripherals can be connected in two ways using a “current limiting resistor” between the atmega328 PINs and the LED(s).
> Which alternative lights up the LED when the output pin is LOW and why?

The LED will light up when the output pin is LOW if the LED is connected in a "sinking" configuration (also called active-low).
There are two ways to connect an LED with a current-limiting resistor to an Arduino pin:

<table>
	<tr><th>Active-High (Source Current Configuration)</th>
	<th>Active-Low (Sink Current Configuration)</th></tr>
	<tr>
	<td><ul><li>
		LED anode (+) → Arduino pin
	</li>
	<li>LED cathode (-) → Resistor → GND</li>
	<li>LED lights up when the pin is HIGH (provides current)</li></ul></td>
	<td><ul><li>LED anode (+) → VCC (5V)</li><li>LED cathode (-) → Resistor → Arduino pin</li><li>LED lights up when the pin is LOW (creates a path to GND, sinking current)</li></ul></td>
	</tr>
</table>

**Why does the LED light up when the pin is LOW?**

In the sinking configuration, setting the pin LOW allows current to flow from VCC (5V) through the LED to the pin, which acts as a path to ground.
The current flows, and the LED turns ON.
When the pin is set HIGH, no voltage difference exists across the LED, so it stays OFF.

This method is commonly used in circuits where multiple LEDs share a common positive voltage (VCC) and are individually controlled by grounding the cathode through a microcontroller pin.

## Traffic Lights

> Traffic light: use 3 Coloured LEDs to create a traffic light

<code>int carRed = 12;    // Car traffic light pins
int carYellow = 11;
int carGreen = 10;
int shortDelay = 200;  // 200ms delay
int longDelay = 500;   // 500ms delay
void setup() {
  pinMode(carRed, OUTPUT);
  pinMode(carYellow, OUTPUT);
  pinMode(carGreen, OUTPUT);
  digitalWrite(carGreen, HIGH);  // Start with green light ON
}
void loop() {
  changeLights();
}
void changeLights() {
  // Green → Yellow → Red
  digitalWrite(carGreen, LOW);   // Turn off Green
  digitalWrite(carYellow, HIGH); // Turn on Yellow
  delay(shortDelay);
  digitalWrite(carYellow, LOW);  // Turn off Yellow
  digitalWrite(carRed, HIGH);    // Turn on Red
  delay(longDelay);
  // Red → Yellow → Green
  digitalWrite(carRed, LOW);     // Turn off Red
  digitalWrite(carYellow, HIGH); // Turn on Yellow
  delay(shortDelay);
  digitalWrite(carYellow, LOW);  // Turn off Yellow
  digitalWrite(carGreen, HIGH);  // Turn on Green
  delay(longDelay);
}
</code>

## Pushbutton

<details>
  <summary>Source</summary>
	
 <a href="https://docs.arduino.cc/language-reference/en/functions/digital-io/digitalread/">Arduino Docs</a>  
  </details>
  
> when the button is pushed, the input pin will go to LOW

<code>int btn = 2;   // Button connected to pin 2
int led = 13;  // LED connected to pin 13
void setup() {
  pinMode(btn, INPUT_PULLUP); // Use internal pull-up resistor
  pinMode(led, OUTPUT);
}
void loop() {
  if (digitalRead(btn) == LOW) { // Button is pressed
    digitalWrite(led, HIGH);     // Turn LED ON
  } else {
    digitalWrite(led, LOW);      // Turn LED OFF
  }
}
</code>

<table>
	<tr><th>When OUTPUT is HIGH(button is not pressed, led is OFF)</th>
	<th>When OUTPUT is LOW(button is pressed, led is ON)</th></tr>
	<tr><td><img width=400 height=auto src='https://github.com/user-attachments/assets/c6e62f20-07f4-4aaf-abcc-6c7b54b693aa'></td>
	<td><img width=400 height=auto src='https://github.com/user-attachments/assets/2bdc9f12-338e-48fa-80ae-7fae1783456a'>
</td></tr>
</table>

<code>int ledPin = 2; // LED connected to digital pin 2
int inPin = 7; // pushbutton connected to digital pin 7
int val = 0; // variable to store the read value
void setup() {
pinMode(ledPin, OUTPUT); // sets the digital pin 2 as output
pinMode(inPin, INPUT); // sets the digital pin 7 as input
}
void loop() {
val = digitalRead(inPin); // read the input pin
digitalWrite(ledPin, val); // sets the LED to the button's value
}
</code>

> Button not pressed: pin 7 will read HIGH and LED will stay OFF.
> 
> Button pressed: pin 7 will read LOW, and the LED will turn ON.
> 
> The "Latch" function in simulation tools like UnoArduSim is typically used to simulate the button being pressed without having to physically hold it down.
> 
> With Latch: The button pin will act like it’s being held down (pressed) without needing to continuously press it.
> When you enable the latch, the button state >will stay LOW until you manually reset it or disable the latch.

## Question 2

> What combinations of the LED resistor configuration and the pushbutton rising/falling signal make the LED light up?

**LED Resistor Configuration:**

<ins>Current-limiting resistor:</ins> This resistor is placed in series with the LED to prevent excessive current from damaging the LED. The value of this resistor depends on the operating voltage of the LED and the voltage provided by the Arduino pin.

The LED will only light up when there is a voltage difference across it, with the correct current flowing through it.

**Pushbutton Signal:**

<ins>Rising edge:</ins> A transition from LOW to HIGH. This occurs when the button is released (assuming an active-low configuration).

<ins>Falling edge:</inss> A transition from HIGH to LOW. This occurs when the button is pressed (again assuming an active-low configuration).

**Button Signal Configurations:**

<ins>Active-Low (Most Common for Buttons):</ins>

In this configuration, the input pin is HIGH when the button is not pressed (due to a pull-up resistor), and the input pin goes LOW when the button is pressed.

<ins>Active-High (Less Common):</ins>

In this configuration, the input pin is LOW when the button is not pressed, and the input pin goes HIGH when the button is pressed.

**Combinations of Pushbutton Signal and LED Resistor Configurations:**

<ins>Active-Low Button (Button Connects to GND when Pressed):</ins>

In this case, we will use an internal pull-up resistor (<code>INPUT_PULLUP</code>) on the input pin.

Button **not pressed:** Pin reads **HIGH** (because the internal pull-up resistor pulls it to **HIGH**).

Button **pressed:** Pin reads **LOW** (because the button connects the pin to **GND**).

**LED Behavior:**

<table>
	<tr><th>    With Rising Edge (Button Released):
</th>
	<th>With Falling Edge (Button Pressed):</th></tr>
	<tr><td> The LED will turn ON when the input pin transitions from LOW to HIGH (button is released), assuming that the LED is connected to the pin that will turn HIGH on release.</td><td>The LED will turn ON when the input pin transitions from HIGH to LOW (button is pressed), assuming that the LED is connected to the pin that will turn LOW on press.</td></tr>
</table>

**Circuit Example (Active-Low):**

<code>int ledPin = 2; // LED connected to digital pin 2
int btnPin = 7; // Button connected to digital pin 7
void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(btnPin, INPUT_PULLUP); // Use internal pull-up resistor
}
void loop() {
  if (digitalRead(btnPin) == LOW) { // Button is pressed (LOW)
    digitalWrite(ledPin, HIGH);     // Turn on the LED
  } else {
    digitalWrite(ledPin, LOW);      // Turn off the LED
  }
}
</code>

> LED lights up when button is pressed, because the button is active-low (pin reads LOW when pressed).

**Active-High Button (Button Connects to VCC when Pressed):**

In this case, we do not use the internal pull-up resistor, so the input pin will need a pull-down resistor to ensure a known **LOW** state when the button is not pressed.

Button **not pressed:** Pin reads **LOW** (due to the pull-down resistor).

Button **pressed:** Pin reads **HIGH** (button connects pin to VCC).

**LED Behavior:**

<table>
	<tr><th>With Rising Edge (Button Pressed):</th><th>With Falling Edge (Button Released):</th></tr>
	<tr><td>The LED will turn ON when the input pin transitions from LOW to HIGH (button is pressed), assuming the LED is connected to the pin that turns HIGH on button press.</td><td>The LED will turn ON when the input pin transitions from HIGH to LOW (button is released), assuming the LED is connected to the pin that turns LOW on button release.</td></tr>
</table>

**Circuit Example (Active-High):**

<code>int ledPin = 2; // LED connected to digital pin 2
int btnPin = 7; // Button connected to digital pin 7
void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(btnPin, INPUT); // Use external pull-down resistor to keep pin LOW when button is not pressed
}
void loop() {
  if (digitalRead(btnPin) == HIGH) { // Button is pressed (HIGH)
    digitalWrite(ledPin, HIGH);      // Turn on the LED
  } else {
    digitalWrite(ledPin, LOW);       // Turn off the LED
  }
}
</code>

>LED lights up when the button is pressed, because the button is active-high (pin reads HIGH when pressed).

**Summary of LED Light-Up Combinations:**
<table>
	<tr><th>Button Configuration</th><th>Button Behavior</th><th>LED Behavior (Rising/Falling Edge)</th></tr>
	<tr><td>Active-Low (with pull-up)</td><td>Button pressed = LOW</td><td>Rising Edge (Release): LED ON when released
Falling Edge (Press): LED ON when pressed</td></tr>
	<tr><td>Active-High (with pull-down)</td><td>Button pressed = HIGH</td><td>Rising Edge (Press): LED ON when pressed
Falling Edge (Release): LED ON when released</td></tr>
</table>

**Key Points to Remember:**

<ul>
<li>Active-Low is more common in button circuits, using internal pull-up resistors.</li>

<li>Active-High can be used with a pull-down resistor for defining the LOW state when the button is not pressed.</li>

<li>The LED turns on depending on whether the input pin goes HIGH or LOW based on the button's state.</li>
</ul>

## Question 3

>“Properties of Pins Configured as INPUT”. What happens to the PIN voltage level if you have nothing connected to it?

**Floating Pin:**

When a pin is configured as **INPUT** and nothing is connected to it, the pin is **"floating"**.

This means that the voltage on the pin is <ins>undefined</ins> and can fluctuate randomly due to electrical noise or capacitance in the environment.

As a result, the pin will read unpredictable values—sometimes **HIGH**, sometimes **LOW**.

## Question 4

> What is the “INPUT_PULLUP” option?

The <code>INPUT_PULLUP</code> option in Arduino configures a pin as an input with an internal pull-up resistor enabled.

**Internal Pull-Up Resistor:** It connects the input pin to **HIGH** (usually 5V) by default through an internal resistor (typically 20kΩ to 50kΩ).

**Pin Behavior:**

When the pin is not connected or floating, it will read **HIGH** (because of the pull-up resistor).
When the pin is connected to **GND** (e.g., when a button is pressed), it will read **LOW**.

It eliminates the need for an external resistor to keep the input pin in a defined state when no button or sensor is connected. It's especially useful for buttons or switches that connect the pin to **GND** when pressed.

<code>pinMode(buttonPin, INPUT_PULLUP);  // Use internal pull-up resistor</code>

> This ensures the pin will be HIGH when the button is not pressed and LOW when the button is pressed (connected to GND).

#### Using Two Pushbuttons

In programming, variables are used for temporary storage of data. They loose their values when the program finishes, typically in an embedded environment this means the microcontroller is reset or powered off.
In the Arduino programming language, integer-type variables are used primarily for storing **16-bit** whole numbers. The most commonly used integer type in Arduino is int.

> Write a program which reads two pushbuttons and controls one LED. 
> Note: // The input pins shall be latched and use pull-down resistors (transition to high)
> //the output LED shall have resistor to Ground

**LED lights up when both buttons are pressed (AND condition)**

<code>int btn1 = 7; // Button 1 connected to pin 7
int btn2 = 6; // Button 2 connected to pin 6
int ledPin = 2; // LED connected to pin 2
void setup() {
  pinMode(btn1, INPUT); // Button 1 with pull-down resistor
  pinMode(btn2, INPUT); // Button 2 with pull-down resistor
  pinMode(ledPin, OUTPUT); // LED pin as output
}
void loop() {
  // Read button states
  int stateBtn1 = digitalRead(btn1);
  int stateBtn2 = digitalRead(btn2);
  // If both buttons are pressed, light up the LED
  if (stateBtn1 == HIGH && stateBtn2 == HIGH) {
    digitalWrite(ledPin, HIGH); // Turn on LED
  } else {
    digitalWrite(ledPin, LOW); // Turn off LED
  }
}
</code>

> The LED shall light up either when both buttons have been pressed, or only one of them has been pressed.

**Truth table**

<table border="1">
  <thead>
    <tr>
      <th>Button 1</th>
      <th>Button 2</th>
      <th>LED (AND Condition)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>LOW</td>
      <td>LOW</td>
      <td>HIGH</td> <!-- LED on when both buttons are pressed -->
    </tr>
    <tr>
      <td>LOW</td>
      <td>HIGH</td>
      <td>LOW</td> <!-- LED off if button 2 is not pressed -->
    </tr>
    <tr>
      <td>HIGH</td>
      <td>LOW</td>
      <td>LOW</td> <!-- LED off if button 1 is not pressed -->
    </tr>
    <tr>
      <td>HIGH</td>
      <td>HIGH</td>
      <td>LOW</td> <!-- LED off if neither button is pressed -->
    </tr>
  </tbody>
</table>


**LED lights up when one or both buttons are pressed (OR condition)**

<code>int btn1 = 7; // Button 1 connected to pin 7
int btn2 = 6; // Button 2 connected to pin 6
int ledPin = 2; // LED connected to pin 2
void setup() {
  pinMode(btn1, INPUT); // Button 1 with pull-down resistor
  pinMode(btn2, INPUT); // Button 2 with pull-down resistor
  pinMode(ledPin, OUTPUT); // LED pin as output
}
void loop() {
  // Read button states
  int stateBtn1 = digitalRead(btn1);
  int stateBtn2 = digitalRead(btn2);
  // If either button is pressed, light up the LED
  if (stateBtn1 == HIGH || stateBtn2 == HIGH) {
    digitalWrite(ledPin, HIGH); // Turn on LED
  } else {
    digitalWrite(ledPin, LOW); // Turn off LED
  }
}
</code>

**Truth table**

<table border="1">
  <thead>
    <tr>
      <th>Button 1</th>
      <th>Button 2</th>
      <th>LED (OR Condition)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>LOW</td>
      <td>LOW</td>
      <td>HIGH</td> <!-- LED on when either button 1 or button 2 is pressed -->
    </tr>
    <tr>
      <td>LOW</td>
      <td>HIGH</td>
      <td>HIGH</td> <!-- LED on if button 2 is pressed -->
    </tr>
    <tr>
      <td>HIGH</td>
      <td>LOW</td>
      <td>HIGH</td> <!-- LED on if button 1 is pressed -->
    </tr>
    <tr>
      <td>HIGH</td>
      <td>HIGH</td>
      <td>HIGH</td> <!-- LED on if both buttons are pressed -->
    </tr>
  </tbody>
</table>

## Serial Monitor

**8 Data Bits, 1 Stop Bit, No Parity Bit (8N1)**

This describes the frame format used in serial communication.

<ins>8 Data Bits</ins> → Each transmitted data packet consists of 8 bits (1 byte).

<ins>1 Stop Bit</ins> → A stop bit is used to signal the end of a transmission (it allows the receiver to know when a new data frame starts).

<ins>No Parity Bit</ins> → Parity is used for error checking, but in this case, it is not used (hence, "No Parity").

> "8N1" is a common serial format, meaning:

<code>[Start Bit] + 8 Data Bits + [No Parity] + 1 Stop Bit</code>

**Baud Rate vs. Bit Speed**

These terms relate to the speed of communication between devices.

<ins>Baud Rate</ins> → The number of symbols (signals) transmitted per second.

<ins>Bit Speed</ins> → The number of bits transmitted per second.

**Why Are They Different?**

If each symbol represents one bit, then baud rate = bit speed.

Some communication protocols use multiple bits per symbol, making the bit speed higher than the baud rate.

> Note: In Arduino Serial Communication, baud rate (e.g., 9600 baud) typically equals bit speed (9600 bits per second).

**How This Affects UnoArduSim?**

When configuring the Serial Monitor in UnoArduSim, you need to set:
<ul>
	<li>Baud rate (must match Serial.begin(baudRate); in code).
</li>
	<li>8N1 as the frame format (default for Arduino).
</li>
	<li>Make sure to open the Serial Monitor after uploading the code.
</li>
</ul>

**Example:**

<code>void setup() {
Serial.begin(9600);
}
void loop() {
Serial.print("Hello World!");
delay(1000);
}
</code>

**Explanation**
<table>
	<tr><td>
	<code>void setup() {
  Serial.begin(9600);
}
</code>	
	</td>
 <td><code>Serial.begin(9600);</code> initializes the serial communication between the Arduino and your computer.
9600 is the baud rate, meaning the communication speed is 9600 bits per second.
This allows data to be sent and received via the Serial Monitor </td></tr>
	<tr><td><code>void loop() {
  Serial.print("Hello World!");
  delay(1000);
}
</code></td>
	<td><code>Serial.print("Hello World!");</code> sends the text <code>"Hello World!"</code> to the Serial Monitor.
<code>delay(1000); </code>pauses execution for 1000 milliseconds (1 second) before repeating.</td></tr>
</table>


#### Transmitting serial data

> Print a single symbol:

<table>
	<tr><th>Baude Rate 1200</th><th>Baude Rate 9600</th></tr>
	<tr><td> <img width=300 height=auto src='https://github.com/user-attachments/assets/242b8eb0-eddb-40cc-813e-72bb8746248e'>
</td>
	<td><img width=300 height=auto src='https://github.com/user-attachments/assets/74b01254-2d42-4ed1-983c-3d1e5b29768e'></td></tr>
	<tr><td><code>int a = 'a';
void setup(){
Serial.begin(1200);
}
void loop(){
Serial.print(a);
delay(2000);
}</code></td><td><code>int a = 'a';
void setup(){
Serial.begin(9600);
}
void loop(){
Serial.print(a);
delay(2000);
}</code></td></tr>
</table>

**Idle State (HIGH)**

The TX (transmit) pin sits at HIGH (1) when no data is being sent. This is the default idle state of UART communication.

**Start Bit (LOW)**

When sending a character like 'a', serial communication starts by sending a start bit, which is always LOW (0).

**Data Bits (LSB First)**

The binary representation of 97 ('a') is 0110 0001.
But because UART sends data LSB (Least Significant Bit) first, the order is 1000 0110.

**Stop Bit (HIGH)**

After sending the data bits, a stop bit (always HIGH) is transmitted to signal the end of the byte.

**How to Calculate the Time for a Single Bit?**

The time for a single bit in serial communication is determined by the baud rate (bits per second). The formula is:

Bit time = 1/Baud rate

This gives us the duration of one bit in seconds.

**Time for a Single Bit at 1200 baud**
   
Bit time = 1/1200​ seconds

=0.0008333 seconds=0.8333 milliseconds=833.3 microseconds
=0.0008333 seconds=0.8333 milliseconds=833.3 microseconds

👉 Each bit lasts 833.3 µs (microseconds) at 1200 baud.

**Time for a Single Bit at 9600 baud**

Bit time = 1/9600 seconds

=0.0001042 seconds=0.1042 milliseconds=104.2 microseconds

👉 Each bit lasts 104.2 µs (microseconds) at 9600 baud.

**What Does This Mean?**

At 1200 baud, bits take **longer** to transmit (833.3 µs per bit), making them easier to observe in the waveform.

At 9600 baud, bits are much **faster** (104.2 µs per bit), making them harder to see.

#### Receiving serial data

<code>void setup(){
Serial.begin(9600);
Serial.setTimeout(10000);
}
void loop(){
Serial.println("Enter data:");
while (Serial.available() == 0){
} //wait for data available
String teststr = Serial.readString(); //read until timeout
teststr.trim(); // remove any \r \n whitespace at the end of the String
if (teststr == "red"){
Serial.println("A primary color");
}else{
Serial.println("Something else");
}
}</code>

This code reads a string input from the Serial Monitor and checks if the user typed "red". If "red" is received, it prints "A primary color"; otherwise, it prints "Something else".

1. <code>Serial.setTimeout(10000);</code>

This function sets the timeout for serial reading functions (like <code>Serial.readString()</code>).

10000 means 10 seconds. If no data is received within this time, <code>Serial.readString()</code> will return whatever it has received so far (even if it's incomplete).

Without this, <code>readString()</code> could hang indefinitely if no data arrives.

2. <code>while (Serial.available() == 0) { } // wait for data</code>

This waits for the user to enter data.

The program freezes here until at least one character is received in the Serial buffer.

3. <code>String teststr = Serial.readString();</code>

Reads the entire available input until a timeout occurs or the user presses Enter (<code>\n</code> or <code>\r</code>).

Since <code>Serial.setTimeout(10000)</code> is used, if the user types nothing for 10 seconds, <code>Serial.readString()</code> will return an empty string.

4. <code>teststr.trim();</code>
Removes any extra whitespace, <code>\r</code>, or <code>\n</code> from the input.

Why? When pressing Enter, Serial Monitor sends "\n" or "\r\n", which could mess up the comparison (teststr == "red").

5. Checking the Input
<code>if (teststr == "red") {
    Serial.println("A primary color");
} else {
    Serial.println("Something else");
}
</code>

If the user typed "red", it prints "A primary color".

Otherwise, it prints "Something else".

> What does the Serial.setTimeout() function call achieve? Is there an unnecessary delay in the code?

Yes, there is a potential delay:

<code>Serial.setTimeout(10000);</code> could cause up to 10 seconds of waiting if the user types nothing.

This is unnecessary if we assume the user will always press Enter after typing.

A lower timeout (e.g., 500 ms) would work better.

**Optimized Code (Non-Blocking Approach)**

<code>void setup() {
    Serial.begin(9600);
    Serial.setTimeout(500);  // Reduced timeout to 500ms instead of 10 seconds
}
void loop() {
    Serial.println("Enter data:");
    if (Serial.available() > 0) {  // Only proceed if there is input
        String teststr = Serial.readString();
        teststr.trim();  // Remove whitespace and newline characters
        if (teststr == "red") {
            Serial.println("A primary color");
        } else {
            Serial.println("Something else");
        }
    }
    delay(500);  // Small delay to avoid spamming Serial Monitor
}
</code>

## Question 5

>  what the readStringUntil() method does?

The Serial.readStringUntil(char terminator) method reads characters from the serial buffer until:

The specified terminator character is encountered.

The timeout <code>(Serial.setTimeout())</code> expires.



## Traffic Lights with pedestrian

<details>
  <summary>Source</summary>
	
 <a href="https://www.dfrobot.com/blog-595.html">Arduino Docs</a>  
  </details>

<code>int carRed = 12; //assign the car lights
	int carYellow = 11;
	int carGreen = 10;
	int button = 9; //button pin
	int pedRed = 8; //assign the pedestrian lights
	int pedGreen = 7;
	int crossTime =5000; //time for pedestrian to cross
	unsigned long changeTime;//time since button pressed
 void setup() {
	pinMode(carRed, OUTPUT);
	pinMode(carYellow, OUTPUT);
	pinMode(carGreen, OUTPUT);
	pinMode(pedRed, OUTPUT);
	pinMode(pedGreen, OUTPUT);
	pinMode(button, INPUT);
	digitalWrite(carGreen, HIGH); //turn on the green lights
	digitalWrite(pedRed, HIGH);
	}
 	void loop() {
	int state = digitalRead(button);
	//check if button is pressed and it is over 5 seconds since last button press
	if(state == HIGH && (millis() - changeTime)> 5000){
	//call the function to change the lights
	changeLights();
	}
	}
 void changeLights() {
	digitalWrite(carGreen, LOW); //green off
	digitalWrite(carYellow, HIGH); //yellow on
	delay(2000); //wait 2 seconds
 digitalWrite(carYellow, LOW); //yellow off
	digitalWrite(carRed, HIGH); //red on
	delay(1000); //wait 1 second till its safe
 digitalWrite(pedRed, LOW); //ped red off
	digitalWrite(pedGreen, HIGH); //ped green on
	delay(crossTime); //wait for preset time period
 	//flash the ped green
	for (int x=0; x<10; x++) {
	digitalWrite(pedGreen, HIGH);
	delay(250);
	digitalWrite(pedGreen, LOW);
	delay(250);
	}
 digitalWrite(pedRed, HIGH);//turn ped red on
	delay(500);
 digitalWrite(carRed, LOW); //red off
	digitalWrite(carYellow, HIGH); //yellow on
	delay(1000);
	digitalWrite(carYellow, LOW); //yellow off
	digitalWrite(carGreen, HIGH); 
 changeTime = millis(); //record the time since last change of lights
	//then return to the main program loop
	}
 </code>


