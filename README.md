# arduino-projects-code

https://www.dfrobot.com/blog-595.html

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

  ## Traffic Lights with pedestrian

 <code></code>
