#  8051

**-------------------------------------------------------------------LED------------------------------------------------------------------**
```
#include&lt;reg51.h&gt;
void MSDelay(unsigned int);
void main ()
{
while(1)
{
P2=0x00;
MSDelay(200);
P2=0xFF;
MSDelay(250);
}
}
void MSDelay(unsigned int itime)
{
unsigned int i,j;
for(i=0;i&lt;itime;i++)
for(j=0;j&lt;1000;j++);
}
```

**-------------------------------------------------------------------LED WITH TIMER-------------------------------------------------**

```
#include&lt;reg51.h&gt;
void MSDelay();
void main ()
{
while(1)
{
P2=0x00;
MSDelay();
P2=0xFF;
MSDelay();
}
}
void MSDelay()
{
TMOD=0x01;
TL0=0xFD;
TH0=0x4B;
TR0=1;
while(TF0==0);
TR0=0;
TF0=0;
}
```

**----------------------------------------------------------------------LCD---------------------------------------------------------------**
```
#include &lt;reg51.h&gt;
sfr ldata = 0x80;
sbit rs = P3^2;
sbit rw = P3^1;
sbit en = P3^3;

void MSDelay(unsigned int itime){
unsigned int i, j;
for(i=0; i&lt;itime;i++)
for(j=0;j&lt;1275;j++);

}
void lcdcmd(unsigned char value) {
ldata = value;
rs = 0;
rw = 0;
en = 1;
MSDelay(1);
en = 0;
return;
}

void lcddata(unsigned char value){
ldata = value;
rs = 1;
rw = 0;
en = 1;
MSDelay(1);
en = 0;
return;
}

void main()
{
while(1)
{
lcdcmd(0x38);
MSDelay(250);
lcdcmd(0x0E);
MSDelay(250);

lcdcmd(0x01);
MSDelay(250);
lcdcmd(0x80);
MSDelay(250);
lcddata(&#39;M&#39;);
MSDelay(250);
lcddata(&#39;O&#39;);
MSDelay(250);
lcddata(&#39;D&#39;);
MSDelay(250);
lcddata(&#39;E&#39;);
MSDelay(250);
lcddata(&#39;R&#39;);
MSDelay(250);
lcddata(&#39;N&#39;);
}
}
```

***--------------------------------------------------------seven segment display-----------------------------------------------------***
```
#include &lt;REG51.h&gt;
const unsigned char Display[10]={0xF6,0x90,0xE5,0xB5,0x93,0x37,0x77,0x94,0xF7,0xB7};
void delaymS(unsigned char x)
{
unsigned int i,j;
for(i=0;i&lt;x;i++)
for(j=0;j&lt;1000;j++);
}
void main()
{
int i=0;
unsigned char x=1;
P0 = 0x00;
while(1)
{
for(i=0;i&lt;10;i++)
{
P0 = ~Display[i];
delaymS(100);
}
}
}
```

**------------------------------------------------Stepper motor clockwise--------------------------------------------------**

```#include &lt;REG51.h&gt;
unsigned int STEP[]={8,1,2,4};
void DelayMs(unsigned int time)
{
int i,j;
for(i=0;i&lt;time;i++)
{
for(j=0;j&lt;50;j++);
}
}
void main()
{
unsigned int i;
while(1)
{
for(i=0;i&lt;=3;i++)

{
P1 = (STEP[i]);
DelayMs(5);
P1 = 0x0F;
DelayMs(5);
}

}

}
```

**------------------------------------------------Stepper motor anti-clockwise--------------------------------------------------**
```
#include &lt;REG51.h&gt;
unsigned int STEP[]={8,1,2,4};
void DelayMs(unsigned int time)
{
int i,j;
for(i=0;i&lt;time;i++)
{
for(j=0;j&lt;50;j++);
}
}

void main()
{
unsigned int i;
while(1)
{
for(i=0;i&lt;=3;i++)

{
P1 = (STEP[3-i]);
DelayMs(5);
P1 = 0x0F;
DelayMs(5);
}

}

}
```

#  Proteus

**LED Proteus**
```
#include <MSP430.h>

int main (void)
 { 
   WDTCTL=WDTPW | WDTHOLD;
   P1DIR=0x01;
   unsigned int i;
   while (1)
   {
   if (P2IN)
   {
  
   P1OUT=0x00;
   for(i=0;i<10000;i++);
   }
   else
   {
    P1OUT=0X01;
   for(i=0;i<10000;i++);
   }
   
   }
     
   return 0;
 }
 ```
 
 **Motor**
 ```
 #include <MSP430.h>
void cw(void);
void acw(void);
int main (void)
 { 
 
 P2DIR=0XFF;
   
   while (1)
      {
      if (P1IN)
        {
          cw();
        }
      else
        {
         acw();
         }
       
      }
          
   return 0;
 }   
void cw(void)
{
  P2OUT=~BIT7;
  __delay_cycles(100000);
  P2OUT=BIT6;
  __delay_cycles(100000);
}
void acw(void)
{
    P2OUT=BIT7;
  __delay_cycles(100000);
  P2OUT=~BIT6;
  __delay_cycles(100000);
}
```

**PWM**
```
#include <MSP430.h>
int main(void)
 {
    WDTCTL = WDTPW | WDTHOLD;
    P1DIR = BIT6; 
    P1SEL = BIT6; 
    TA0CCR0 = 1000; 
    TA0CCTL1 = OUTMOD_7;
    TA0CCR1 = 750; 
    TACTL = TASSEL_2 + MC_1; 
    __bis_SR_register(LPM0_bits); 
    return 0;
}
```
