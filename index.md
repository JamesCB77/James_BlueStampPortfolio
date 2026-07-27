# BlueStamp Chess Clock
Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| James C | Herricks High School | Engineering (not sure rn) | Incoming Junior

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

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone 

# First Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/fApPMsWDlFM?si=QODlaHQl--UL4WQF" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For your first milestone, describe what your project is and how you plan to build it. You can include:
- An explanation about the different components of your project and how they will all integrate together
- Technical progress you've made so far
- Challenges you're facing and solving in your future milestones
- What your plan is to complete your project
My project is a Chess clock/timer. For my first milestone I wanted to make the the physical Chess clock. Because of this most of my time revolved on the hardware part. I was able to set up all the pieces needed and connect the wires. A small mistake I had was I mostly mixed up the active and passive buzzer. The main part of this milestone I spent was definitely connecting all the wires. This was my first time working with a breadboard and I had to learn the right way to connect everything and make sure there wasn't gonna be a short circuit ot something like that. It was also very messy with all the wires everywhere so I tried rearranging the wires in better places to make it easier for me. I hope to add the code and make sure it all works in the second Milestone. 

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Code for the Chess clock
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
#include <TM1637Display.h>

//encoder
#define outputA 2
#define  outputB 3
#define enSw 4

// 4-digital display pins (Digital Pins)
#define  CLKA 5
#define DIOA 6
#define CLKB 7
#define DIOB 8
TM1637Display displayA(CLKA,  DIOA);
TM1637Display displayB(CLKB, DIOB);


//button, led, buzzer
#define  btA 9
#define btB 10
#define buzz 11

//
uint8_t segOff[]={false,false};
//
  int timerSet = 0; 
 int aState;
 int aLastState;
 int modeState=0;  
  int modeStateLast=0;

unsigned long minSetA = 5;
unsigned long secSetA  = 0;
unsigned long minSetB = 5;
unsigned long secSetB = 0;
unsigned long  timeO = 0;
unsigned long timeA = 0;
unsigned long timeB = 0;
int enSwStatus=1;
int  enSwStatusLast=1;
int btAStatus=1;
int btAStatusLast=1;
int btBStatus=1;
int  btBStatusLast=1;

void setup() {
 Serial.begin(9600);
 
 displayA.setBrightness(4);
  displayA.clear();
 displayB.setBrightness(4);
 displayB.clear();
 delay(1000);
  
 pinMode(btA,INPUT_PULLUP);
 pinMode(btB,INPUT_PULLUP);
 pinMode(buzz,OUTPUT);

  pinMode (outputA,INPUT);
 pinMode (outputB,INPUT);
 pinMode(enSw,INPUT_PULLUP);
  aLastState = digitalRead(outputA);  
 digitalWrite(buzz,LOW); 

}

void  loop() {
  switch(modeState){
    
    case 0:
      digitalWrite(buzz,LOW);
      displayA.showNumberDecEx(minSetA*100 + secSetA, 0b01000000, true, 4, 0);
      displayB.showNumberDecEx(minSetB*100 + secSetB, 0b01000000, true, 4, 0);
      enSwStatus=digitalRead(enSw);
      btAStatus=digitalRead(btA);
      btBStatus=digitalRead(btB);
      if(enSwStatus==0&&enSwStatusLast==1){
        modeState=1;
      }
      if(btAStatus==0&&btAStatusLast==1){
        modeState=12;
        timeO=  millis();
        timeA=0;
        timeB=0;
      }
      if(btBStatus==0&&btBStatusLast==1){
        modeState=11;
        timeO= millis();
        timeA=0;
        timeB=0;
      }
      btAStatusLast=btAStatus;
      btBStatusLast=btBStatus;
      enSwStatusLast=enSwStatus;
      break;
      
    case 1:
      enSwStatus=digitalRead(enSw);
      minSetA=setNum(minSetA);
      displayBlink("A",0);
      if(enSwStatus==0&&enSwStatusLast==1){
        modeState=2;
      }
      enSwStatusLast=enSwStatus;
      break;
    case 2:
      enSwStatus=digitalRead(enSw);
      secSetA=setNum(secSetA);
      displayBlink("A",2);
      if(enSwStatus==0&&enSwStatusLast==1){
        modeState=3;
      }
      enSwStatusLast=enSwStatus;
      break;
    case 3:
      enSwStatus=digitalRead(enSw);
      minSetB=setNum(minSetB);
      displayBlink("B",0);
      if(enSwStatus==0&&enSwStatusLast==1){
        modeState=4;
      }
      enSwStatusLast=enSwStatus;
      break;
    case 4:
      enSwStatus=digitalRead(enSw);
      secSetB=setNum(secSetB);
      displayBlink("B",2);
      if(enSwStatus==0&&enSwStatusLast==1){
        modeState=0;
      }
      enSwStatusLast=enSwStatus;
      break;
      
    case 11:
      timeA = millis()-timeO-timeB;
      if(timeOut()==1){
        modeState=13;
      }
      displayA.showNumberDecEx(timeDisplay(timeA,"A"),  0b01000000, true, 4, 0);
      displayB.showNumberDecEx(timeDisplay(timeB,"B"),  0b01000000, true, 4, 0);
      btAStatus=digitalRead(btA);
      if(btAStatus==0&&btAStatusLast==1){
        modeState=12;
      }
      btAStatusLast=btAStatus;
      enSwStatus=digitalRead(enSw);
      if(enSwStatus==0&&enSwStatusLast==1){
        modeState=0;
      }
      enSwStatusLast=enSwStatus;
      break;
      
    case 12:
      timeB  = millis()-timeO-timeA;
      if(timeOut()==1){
        modeState=13;
      }
      displayA.showNumberDecEx(timeDisplay(timeA,"A"), 0b01000000,  true, 4, 0);
      displayB.showNumberDecEx(timeDisplay(timeB,"B"), 0b01000000,  true, 4, 0);
      btBStatus=digitalRead(btB);
      if(btBStatus==0&&btBStatusLast==1){
        modeState=11;
      }
      btBStatusLast=btBStatus;
      enSwStatus=digitalRead(enSw);
      if(enSwStatus==0&&enSwStatusLast==1){
        modeState=0;
      }
      enSwStatusLast=enSwStatus;
      break;
    case 13:
      analogWrite(buzz,255);
      delay(100);
      analogWrite(buzz,0);
      delay(100);
      enSwStatus=digitalRead(enSw);
      if(enSwStatus==0&&enSwStatusLast==1){
        modeState=0;
      }
      enSwStatusLast=enSwStatus;
      break;
    
    default:
 
      break;
  }
}

int setNum(int num) { 
   aState = digitalRead(outputA);
   if (aState != aLastState){     
     if (digitalRead(outputB) != aState) {  
       num ++;
     } else {
       if(num>0){
         num --;
       }
     }
   } 
   aLastState = aState;
   return num;
 }
  void displayBlink(String player, int pos){
  if(player=="A"){
     if ((millis()*5/1000)%2==0){
        displayA.setSegments(segOff, 2, pos);
     } else displayA.showNumberDecEx(minSetA*100  + secSetA, 0b01000000, true, 4, 0);
  } else if  (player=="B"){
     if  ((millis()*5/1000)%2==0){
        displayB.setSegments(segOff, 2, pos);
     }  else displayB.showNumberDecEx(minSetB*100 + secSetB, 0b01000000, true, 4, 0);
  }
 }

 long timeDisplay(long timeRun, String player){
    long min_t=0;
    long sec_t=0;
    if (player=="A"){
        min_t= (minSetA*60+secSetA-timeRun/1000)/60;
        sec_t = (minSetA*60+secSetA-timeRun/1000)%60;
    } else if (player=="B"){
      min_t= (minSetB*60+secSetB-timeRun/1000)/60;
      sec_t= (minSetB*60+secSetB-timeRun/1000)%60;
    }
   
  return min_t*100 + sec_t;
 }

 int timeOut(){
  int  isTimeOut=0;
  if (timeA>(minSetA*60+secSetA)*1000){
    isTimeOut=1;
  } else if (timeB>(minSetB*60+secSetB)*1000){
    isTimeOut=1;
  } 
  return  isTimeOut;
 }

```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| UNO R3 Most Complete Starter Kit | kit providing parts: Arduino uno r3, jumper wires, active buzzer, buttons, USB cable | $56.99 | <a href="https://us.elegoo.com/products/elegoo-uno-most-complete-starter-kit?variant=40394045390896&srsltid=AfmBOoqSgfQp84R36vfaww_tAvy6lnrlz0THkj6JrSihUclU8NkhmnbspfA&utm_source=officiallisting&utm_medium=referral&variant=40394045390896&srsltid=AfmBOoqSgfQp84R36vfaww_tAvy6lnrlz0THkj6JrSihUclU8NkhmnbspfA&utm_id=usstore"> Link </a> |
| 4 digital display TM1637 | shows the timer | $7.99 | <a href="https://www.amazon.com/WWZMDiB-Module%EF%BC%8CLED-Brightness-Adjustable-Accessories/dp/B0BFQNFX6D"> Link </a> |
| Rotary Encoder with Push-Button | sets up the timer | $1.49 | <a href="https://envistiamall.com/products/rotary-encoder-module-with-pushbutton-switch-ky-040?currency=USD&country=US&variant=28453729673&utm_source=google&utm_medium=cpc&utm_campaign="> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
