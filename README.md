# MANTY-NXT
Codigo en NXC para el funcionamiento del programa
#include "NXCDefs.h"
int x,y=1;

sub sincronizacion()
{ 
  OnFwd(OUT_BC,75);
  until(Sensor(IN_1)==1);
  OnFwd(OUT_BC,75);
  until(Sensor(IN_1)==0);
  OnFwd(OUT_B,75);
  until(Sensor(IN_1)==1);
  RotateMotor(OUT_B,75,180);
  OnFwd(OUT_C,75);
  until(Sensor(IN_1)==1);
}
task caminar() 
{
  y=0;
  sincronizacion();
 
  while(true)
    {
      OnFwd(OUT_BC,75);
      until(y=1); 
      x=Random(100);
      if(x>50)
        {
          OnFwd(OUT_C,70);
           Wait(3000);
          OnRev(OUT_B,70);
          Wait(3000);
        }
      else
        {
          OnFwd(OUT_B,70);
          Wait(3000);
          OnRev(OUT_C,70);
          Wait(3000);
        }
      y=0;  
      sincronizacion();
 
    } 
}
task cabeza()
{
  while(true)
    {
      OnFwd(OUT_A,20);
      Wait(1500);
      RotateMotor(OUT_A,35,-120);
      while(SensorUS(IN_4)>15)
        {
          RotateMotor(OUT_A,35,50);
          RotateMotor(OUT_A,35,-50);
        }
      y=1;
      OnFwd(OUT_A,20);
      PlayTone(440,500);
      Wait(1500);
      
      while(y==1)
          {
          Wait(1000);
          PlayTone(200,500);
        } 
    }  
}

task main()
{
 SetSensorTouch(IN_1);
 SetSensorLowspeed(IN_4);
 Precedes(cabeza,caminar);
}
