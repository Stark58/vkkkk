//верия для аттини 85
// последнее изменение 6 01 2017
// значение датчика 377

//int analogInPin = 2;  // для датчика вакуума
int sensorValue = 0;    // переменная датчика
//float outputValue = 0;        // value output to the Serial port and LCD display


byte knopka = 2; //12. 3 nano.
int knop = 0;

boolean regim = 0;
boolean xz = 0;
boolean mc = 0;

// светодиоды индикации
byte led1 = 13; //9 pro. 10 nano
int led2 = 6; //8 pro. 12 nano.

// Считывание напряжения с аккумуляторов.
//резисторы делителя напряжения

float vout = 0.0;
float vin = 0.0;
float R1 = 10000.0; // сопротивление R1 (20K)
float R2 = 20000.0; // сопротивление R2 (10K)
int value = 0;

/****************************************************************************
   Главная программа
 ****************************************************************************/

void setup()
{
  pinMode (knopka, INPUT);
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
  
 pinMode(7, OUTPUT);
 digitalWrite(7, 0);
analogWrite(6, 120);
Serial.begin(9600);
 
  digitalWrite(led1, 1);
  //digitalWrite(led2, 1);
  delay(1000);
  digitalWrite(led1, 0);
  //digitalWrite(led2, 0);
  //delay(200);
}

void loop()
{
  sensorValue = analogRead(2);
  knop = digitalRead(knopka);
  delay(50);

  value = analogRead(3);
  vout = (value * 5.0) / 1024.0;
  vin = vout / (R2 / (R1 + R2));
  Serial.println(vin );
  if (vin < 0.09) {
    vin = 0.0; // обнуляем нежелательное значение
  }

  if (vin > 4)
  {
    digitalWrite( led1, 0);
    analogWrite(6, 0);
    //digitalWrite( led2, 1);
    delay(500);
    digitalWrite( led1, 1);
    analogWrite(6, 120);
    //digitalWrite( led2, 0);
     delay(500);
  }
  if (vin < 4 && vin > 3.4)
  {
    //digitalWrite( led1, 1);
    analogWrite(6, 120);
    digitalWrite( led1, 0);
  }
  if (vin < 3.4)
  {
    digitalWrite( led1, 0);
    analogWrite(6, 120);
    delay(1000);
    analogWrite(6, 0);
    digitalWrite( led1, 0);
    delay(1000);
  } 
  
}

/****************************************************************************
   Функции
 ****************************************************************************/
