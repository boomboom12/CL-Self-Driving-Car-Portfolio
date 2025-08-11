# Self Driving Car Project w/ Color Recognition Software
Hi, I'm Christopher and welcome to my portfolio! My project is a Self Driving Car that can operate with a remote control or on it's own with obstacle avoidance modules that allow it to steer clear of any walls or objects that get in it's way! My main modification that I have added is color recognition software, with it being able to recognize different colors and play a certain tone depending on the color it senses.



<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->


| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Christopher L | South Pasadena High School | Mechanical Engineering | Incoming Sophmore

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/JMvCpzSXIag?si=zKpc_6qXy9Ut-ZDU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Hi, my name is Christopher and I'm from South Pasadena High School, and I am excited to what I did in my milestone 2 with you. First, I learned to use the ultrasonic sensor to detect obstacles and even follow my hand, then combined what I learned in the previous steps to make the car drive on its own. I also added remote control using the IRemote library after, which I thought was really cool since it reminded me of playing with RC cars in my childhood. One issue that I faced was that the car veered to one side because one motor was faster, but I fixed it by adjusting the motor speeds so it could finally drive straight. Now I’m planning to add RGB lights to show speed, a top shell, and color detection. Overall, I’m very excited to keep working on it and see where it goes.

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/cfpAFCpazYc?si=DCRhDKv3flhmxadt" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My project, the self driving car, contains a lot of different pieces that work together and allow the car to function. First, it has a main baseplate that all the parts are screwed onto, such as electronic boards like the Arduino Uno, LED object sensors, and a mini breadboard to create circuits that allow each sensor and the motors to connect to the Arduino via wires. So far, I have assembled the entire Arduino, as shown in my milestone video, and have gotten it to move, turn, follow a black line using color sensors, and use the LED object sensors to avoid walls and anything that comes close to the car. A big challenge I am currently facing are that the speed of the motors is not the same when moving forwards or backwards, causing the vehicle to veer left during movement. However, in a later step I will learn how to calibrate the speed of each motor, and I can adjust it as needed to allow them to move at the same speed. Overall, I am excited to finish the tutorial stage and move onto my modifications of the car.

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
#include <IRremote.h>

// TCS230 or TCS3200 pins wiring to Arduino
#define S0 8
#define S1 11
#define S2 2
#define S3 4
#define sensorOut 3
const int SpeakerPin = 13;
const int IR_RECEIVE_PIN = 12;  // Define the pin number for the IR Sensor

const int A_1B = 5;
const int A_1A = 6;
const int B_1B = 9;
const int B_1A = 10;

int speed = 150;

// Stores frequency read by the photodiodes
int redFrequency = 0;
int greenFrequency = 0;
int blueFrequency = 0;

// Stores the red. green and blue colors
int redColor = 0;
int greenColor = 0;
int blueColor = 0;

void setup() {
  // Setting the outputs
  pinMode(S0, OUTPUT);
  pinMode(S1, OUTPUT);
  pinMode(S2, OUTPUT);
  pinMode(S3, OUTPUT);

  
  // Setting the sensorOut as an input
  pinMode(sensorOut, INPUT);
  
  // Setting frequency scaling to 20%
  digitalWrite(S0,HIGH);
  digitalWrite(S1,LOW);
  pinMode(SpeakerPin, OUTPUT);  // Set pin as output
  pinMode(A_1B, OUTPUT);
  pinMode(A_1A, OUTPUT);
  pinMode(B_1B, OUTPUT);
  pinMode(B_1A, OUTPUT);

  //IR remote
  IrReceiver.begin(IR_RECEIVE_PIN, ENABLE_LED_FEEDBACK);  // Start the IR receiver // Start the receiver

 
  
  // Begins serial communication
  Serial.begin(9600);
}

void loop() {
  if (IrReceiver.decode()) {
    //    Serial.println(results.value,HEX);
    String key = decodeKeyValue(IrReceiver.decodedIRData.command);
    if (key != "ERROR") {
      Serial.println(key);

      if (key == "+") {
        speed += 50;
      } else if (key == "-") {
        speed -= 50;
      } else if (key == "2") {
        moveForward(speed);
        delay(1000);
      } else if (key == "1") {
        moveLeft(speed);
      } else if (key == "3") {
        moveRight(speed);
      } else if (key == "4") {
        turnLeft(speed);
      } else if (key == "6") {
        turnRight(speed);
      } else if (key == "7") {
        backLeft(speed);
      } else if (key == "9") {
        backRight(speed);
      } else if (key == "8") {
        moveBackward(speed);
        delay(1000);
      }

      if (speed >= 255) {
        speed = 255;
      }
      if (speed <= 0) {
        speed = 0;
      }
      delay(500);
      stopMove();
    }

    IrReceiver.resume();  // Enable receiving of the next value
  }
  // Setting RED (R) filtered photodiodes to be read
  digitalWrite(S2,LOW);
  digitalWrite(S3,LOW);
  
  // Reading the output frequency
  redFrequency = pulseIn(sensorOut, LOW);
  // Remaping the value of the RED (R) frequency from 0 to 255
  // You must replace with your own values. Here's an example: 
  // redColor = map(redFrequency, 70, 120, 255,0);
  redColor = map(redFrequency, 107, 200, 255,0);
  
  // Printing the RED (R) value
  Serial.print("R = ");
  Serial.print(redColor);
  delay(100);
  
  // Setting GREEN (G) filtered photodiodes to be read
  digitalWrite(S2,HIGH);
  digitalWrite(S3,HIGH);
  
  // Reading the output frequency
  greenFrequency = pulseIn(sensorOut, LOW);
  // Remaping the value of the GREEN (G) frequency from 0 to 255
  // You must replace with your own values. Here's an example: 
  // greenColor = map(greenFrequency, 100, 199, 255, 0);
  greenColor = map(greenFrequency, 73, 265, 255, 0);
  
  // Printing the GREEN (G) value  
  Serial.print(" G = ");
  Serial.print(greenColor);
  delay(100);
 
  // Setting BLUE (B) filtered photodiodes to be read
  digitalWrite(S2,LOW);
  digitalWrite(S3,HIGH);
  
  // Reading the output frequency
  blueFrequency = pulseIn(sensorOut, LOW);
  // Remaping the value of the BLUE (B) frequency from 0 to 255
  // You must replace with your own values. Here's an example: 
  // blueColor = map(blueFrequency, 38, 84, 255, 0);
  blueColor = map(blueFrequency, 100, 250, 255, 0);
  
  // Printing the BLUE (B) value 
  Serial.print(" B = ");
  Serial.print(blueColor);
  delay(2000);

  // Checks the current detected color and prints
  // a message in the serial monitor
  if (redColor < 110 && greenColor < 35 && blueColor < 25) {
      Serial.println(" - BLACK detected!");
      tone(SpeakerPin, 100, 500);  // Play a tone ( [Pin] , [Tone_Frequency] , [Duration_ms] )
  }
  else if (249 < redColor < 270 && greenColor > 300 && 250 < blueColor < 260) {
      Serial.println(" - BROWN detected!");
      tone(SpeakerPin, 200, 500);  // Play a tone ( [Pin] , [Tone_Frequency] , [Duration_ms] )
  }
  else if (redColor > 200 && greenColor > 150 && blueColor > 200) {
      Serial.println(" - WHITE detected!");
      tone(SpeakerPin, 300, 500);  // Play a tone ( [Pin] , [Tone_Frequency] , [Duration_ms] )
  }
  else if (redColor > 30 && greenColor > 135 && blueColor < 110) {
      Serial.println(" - YELLOW detected!");
      tone(SpeakerPin, 400, 500);  // Play a tone ( [Pin] , [Tone_Frequency] , [Duration_ms] )
  }
  else if(redColor > greenColor && redColor > blueColor){
      Serial.println(" - RED detected!");
      tone(SpeakerPin, 500, 500);  // Play a tone ( [Pin] , [Tone_Frequency] , [Duration_ms] )
  }
  else if(greenColor > redColor && greenColor > blueColor){
    Serial.println(" - GREEN detected!");
    tone(SpeakerPin, 600, 500);  // Play a tone ( [Pin] , [Tone_Frequency] , [Duration_ms] )
  }
  else if(blueColor > redColor && blueColor > greenColor){
    Serial.println(" - BLUE detected!");
    tone(SpeakerPin, 700, 500);  // Play a tone ( [Pin] , [Tone_Frequency] , [Duration_ms] )
  }
  else{
    Serial.println("UNKNOWN COLOR");
  }

}

void moveForward(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, speed);
  analogWrite(B_1B, speed);
  analogWrite(B_1A, 0);
}

void moveBackward(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);
}

void turnRight(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, speed);
  analogWrite(B_1A, 0);
}

void turnLeft(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, speed);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);
}

void moveLeft(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, speed);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, 0);
}

void moveRight(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, speed);
  analogWrite(B_1A, 0);
}

void backLeft(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, 0);
}

void backRight(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);
}

void stopMove() {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, 0);
}


String decodeKeyValue(long result)
{
  switch(result){
    case 0x16:
      return "0";
    case 0xC:
      return "1"; 
    case 0x18:
      return "2"; 
    case 0x5E:
      return "3"; 
    case 0x8:
      return "4"; 
    case 0x1C:
      return "5"; 
    case 0x5A:
      return "6"; 
    case 0x42:
      return "7"; 
    case 0x52:
      return "8"; 
    case 0x4A:
      return "9"; 
    case 0x9:
      return "+"; 
    case 0x15:
      return "-"; 
    case 0x7:
      return "EQ"; 
    case 0xD:
      return "U/SD";
    case 0x19:
      return "CYCLE";         
    case 0x44:
      return "PLAY/PAUSE";   
    case 0x43:
      return "FORWARD";   
    case 0x40:
      return "BACKWARD";   
    case 0x45:
      return "POWER";   
    case 0x47:
      return "MUTE";   
    case 0x46:
      return "MODE";       
    case 0x0:
      return "ERROR";   
    default :
      return "ERROR";
    }
}
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Sunfounder Kit | Contains the materials used for building the car, such as wheels, base, motors, Arduino, etc. | $162.99 | <a href="https://www.amazon.com/SunFounder-Compatible-Tutorials-Including-Controller/dp/B0B778L1DZ/ref=sr_1_1?crid=3JQTX3SPFIY9Z&dib=eyJ2IjoiMSJ9.D9LrCZJnua_keVMLJz2FWi87-vYq5Z0c0hghVjdTqVV5SxTVgutlUut8NIgJpkDha5RIUUEOd8ZL_9-liu4TuIX3Y5c9E3mrmlKMD_2d9cnuKu55yBqRD35FcNSR2oUIVkT7byKksfuqXVAx34A8gUuPMYKaM3Jepu1QA3uOutR5sR0O3bugifITwp4OocPwYE4ZDNZaCae7Y3Ydd5zuneo_8PLiYwbdyVH9QvcGEwg.-iuZvwFJywFFRggszeNpXLuAEE8nPtLKbqmhVUOfLc0&dib_tag=se&keywords=sunfounder+3+in+1+starter+kit+for+arduino+uno&qid=1718980379&sprefix=3+in+1+ard%2Caps%2C120&sr=8-1"> Link </a> |
| USB-USBC | Converts USB Type A port into USB Type C port | $12.99 | <a href="https://www.amazon.com/ENVEL-Transfer-Converter-Thunderbolt3-Compatible/dp/B0D3T2QDVJ/ref=sxin_17_pa_sp_search_thematic_sspa?content-id=amzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93%3Aamzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93&crid=2XKXL9JJ62FRH&cv_ct_cx=usba%2Bto%2Busbc&keywords=usba%2Bto%2Busbc&pd_rd_i=B0D3T2QDVJ&pd_rd_r=4d705a77-7d1c-4543-b61d-c95f071f99c3&pd_rd_w=zydwI&pd_rd_wg=Ra4PI&pf_rd_p=70fcaece-2dd2-4653-bf00-fb6af1af1b93&pf_rd_r=GA7XX674ZRQ1VKTH3HWX&qid=1750358432&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=usba%2Bto%2Busb%2Caps%2C106&sr=1-1-e169343e-09af-4d41-85b1-8335fe8f32d0-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&th=1"> Link </a> |
| 9V Batteries | Battery that powers the car | $18.88 | <a href="https://www.amazon.com/Amazon-Basics-Performance-All-Purpose-Batteries/dp/B00MH4QM1S/ref=sr_1_5_pp?crid=3TQ7ANPH958JM&dib=eyJ2IjoiMSJ9.bmcV2Upj_vpB6G9CFlPPxYAryat512da7ekZjc52HecXSTmtx7PbJ50EgQFPCMqlAxjOUq-tL4vQTpozlHvH89bMwx-HJoyGcdz6EY8HrMxahTiqOXkoP7ewkDcgHoMhmHamdlQfW6FBHO0Gm-DYZZnnMuvEU3qOpemA8PGEvRhEx4-lGaBZhrvls039G1-9SizAW-YRGXZ2fFrdVDlREyyOhAuxXZaE5QqUxWesRQgP9UfGOYaInRWTTPwhDbXFa-RPzGbU1C_u4wq-NMqKBtWEQqR9-cA8O3FYOx3icEY.dtKJmI2T-iCmMM_bYnbiHUWzhKpJDRxS-bBmZIwYFKM&dib_tag=se&keywords=9v+batteries&qid=1720651326&rdc=1&s=electronics&sprefix=9v+batteries%2Celectronics%2C105&sr=1-5"> Link </a> |
| Mini Speaker 3W 8Ohm with Dupont Interface for Arduino & Small Projects | Connects to the Arduino to play tones and sounds | $9.99 | <a href="https://www.amazon.com/DWEII-Loundspeaker-Compatible-Motherboard-Electronic/dp/B0CX1JC6NM/ref=sr_1_1_pp?crid=NY57A9F90FCC&dib=eyJ2IjoiMSJ9._rqO-TTnOr5vIdnX_kYmyAREWn7rpS35zhwyI-pUOli0wp1Qa8poE3Y13cWNfk97Mm0csnCTn9bLws5WpSh7XMBngHKur6-O8CJdiUwbLo9gKT7YU-zGi5EvTNednMucFmtZbq3bukw_3Rwa--cx3Ad6G5YBewuyJ6PK-GG--8L1RhdUz8IaT6sdAfb1NKWoqO-msE0OI6eibKzOQzPck7ABuSf3EXnY5Hc2tou7PyY.qeFx8rHujUHNvIpgkwASkYSyu2481woUUwAoUcWXcnM&dib_tag=se&keywords=arduino%2Bspeaker&qid=1754082122&sprefix=arduino%2Bspeake%2Caps%2C111&sr=8-1&th=1"> Link </a> |
| TCS3200 Color Sensor Module for Arduino | Senses and collects RGB values | $8.99 | <a href="https://www.amazon.com/Teyleten-Robot-TCS230-TCS3200-Recognition/dp/B08HH8QYF8/ref=sr_1_1_pp?crid=2TQ38VJOESUU6&dib=eyJ2IjoiMSJ9.62cEBlV7_fWPuL0Go_CdCB9bzY376QMgb3TuhZGPzdw4P9SWvzR3pXt4oGemNlKSDw8Ixqx6qiiY_U5cUrnx3YkIlLSpvUo8kgGqHVG_1ww8Ll3ahWYe_lMv1sYh6gM0L46XPzCESJk6azDtdI4DCd1mVxpVEFd5vbU9zLOQZtH-DjBMMUohLfTHc4bsd-7xCTgjVY_SpY_dOZAaoPDVGpeap5W4h6Vuax5FtadlxmM.4qCnGe_37lxOwjVhaKpOsTnQTmRLbO4QmqhPxzAe8QQ&dib_tag=se&keywords=arduino+color+sensor&qid=1754082140&sprefix=arduino+color+senso%2Caps%2C108&sr=8-1"> Link </a> |


# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
