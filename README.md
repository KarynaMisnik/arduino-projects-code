# arduino-projects-code

* [Traffic Lights](#traffic-lights)
* [Traffic Lights with pedestrian](#traffic-ights-with-pedestrian)

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
 </code>

  ## Traffic Lights with pedestrian

 <code></code>
