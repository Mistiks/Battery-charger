#include <Wire.h> // добавляем необходимые библиотеки
#include <LiquidCrystal.h>

LiquidCrystal lcd(8, 9, 4, 5, 6, 7); 

double voltage = 0; // переменная, хранящая значение напряжения
int circle = 0; // переменная, хранящая число циклов
int counter = 10000; // переменная, используемая для уменьшения частоты вывода информации на экран

void setup() {
  Serial.begin(9600);
  lcd.begin(16, 2); // инициализация экрана (2 строки по 16 символов)
  circle = 0;
}

void loop() {   
  voltage = analogRead(3); //измерение значения напряжения
  voltage = voltage / 204.6 * 5;

  if (voltage > 18)
    { 
        lcd.setCursor(0,0);
        lcd.print("Battery out     ");
        lcd.setCursor(0,1);
        lcd.print("                ");
        return;
    }

   if (voltage < 1)
   { 
        lcd.setCursor(0,0);
        lcd.print("Battery out     ");
        lcd.setCursor(0,1);
        lcd.print("                ");
        return;
    }  

   if (circle == 3) // если тренировка завершена 
  {
    analogWrite(3,0);
    analogWrite(11,0);
    lcd.setCursor(0,0);
    lcd.print("Process complete");
    lcd.setCursor(0,1);
    lcd.print("                ");
    return;
  }

  delay(2000);
  
  if (voltage < 12.8)
  {
    Discharge();
  }
  
  if (voltage >= 12.8)
  {
    Charge();
  }
  
  do 
  {
    voltage = analogRead(3); //измерение значения напряжения
    voltage = voltage / 204.6 * 5;
  
    if (voltage < 11)
    {
      lcd.clear();
      circle++;
      Charge();
      lcd.clear();
      Discharge();
    }  
    

    if (voltage > 14)
    {
      lcd.clear();
      circle++;
      Discharge();
      lcd.clear();
      Charge();
    }
  
  } while (circle != 3);

  if (voltage < 11)
  {
    Charge();
  }
  
}

void Charge() {
  do 
  {
        analogWrite(11,0); // отключить разрядку
        analogWrite(3,255); // включить зарядку
        voltage = analogRead(3);
        voltage = voltage / 204.6 * 5;
        if (counter == 10000) 
        {
          lcd.setCursor(0,0);
          lcd.print("Voltage = "); lcd.print(voltage); lcd.print("V");
          lcd.setCursor(0,1);
          lcd.print("Charging "); lcd.print(circle);
          counter = 0;
        }
        counter++; 
    } while (voltage < 14.8); // повторять, пока напряжение не поднимется до 15
}

void Discharge() {
  do 
  {
        analogWrite(3,0); // отключить зарядку
        analogWrite(11,255); // включить разрядку
        voltage = analogRead(3);
        voltage = voltage / 204.6 * 5;
        if (counter == 10000)
        {
          lcd.setCursor(0,0);
          lcd.print("Voltage = "); lcd.print(voltage); lcd.print("V");
          lcd.setCursor(0,1);
          lcd.print("Discharging "); lcd.print(circle);
          counter = 0;
        }
        counter++;
    } while (voltage > 10.8); // повторять, пока напряжение не снизится до 10.8
}
