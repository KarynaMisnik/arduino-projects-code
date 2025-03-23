# arduino-projects-code

https://www.dfrobot.com/blog-595.html

* [Blinking Led](#blinking-led)
* [Traffic Lights](#traffic-lights)
* [Traffic Lights with pedestrian](#traffic-ights-with-pedestrian)

## Blinking LED

**Code**

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

<table>
	<tr><th>When OUTPUT is HIGH(led is blinking red)</th>
	<th>When OUTPUT is LOW(led is not blinking)</th></tr>
	<tr><td><img width=400 height=auto src='https://github.com/user-attachments/assets/3e117b31-fa0b-4fd1-88d2-8791b6f98796'></td>
	<td><img width=400 height=auto src='https://github.com/user-attachments/assets/1a517274-f4a0-4a09-81f3-9c910e354c25'>
</td></tr>
</table>



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
