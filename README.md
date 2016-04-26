# Termometr-LCD-4x20-DS18B20

//===================================================================================================//
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <OneWire.h>// Инициализация библиотеки шины OneWire.
#include <DallasTemperature.h>// Инициализация библиотеки термодатчиков.
//===================================================================================================//
//#define LCD_I2C_ADDR    63 // Объявляем I2C адрес для микросхемы PCF8574T
#define BACKLIGHT     7 //Х.З.
#define LCD_EN  4 //я незнаю как сказать или как понять
#define LCD_RW  5 //но эти строки нужнв для того чтобы
#define LCD_RS  6 //Ардуино понимала как подключены ножки
#define LCD_D4  0 //дисплея к ножкам I2C микросхемы и все
#define LCD_D5  1  //работает нормально поверьте
#define LCD_D6  2 //проверено мной лично 0259!
#define LCD_D7  3 //если кто сможет обяснить назначение этих строк правильно пожалуйста распишите для меня
#define ONE_WIRE_BUS 10// Подключение цифрового вывода датчика к 10-му пину Ардуино.
//===================================================================================================//
LiquidCrystal_I2C lcd(63, 20, 4); // устанавливаем адрес 63, и дисплей 20 символов в 4 строки (20х4)
OneWire oneWire(ONE_WIRE_BUS);// Запуск интерфейса OneWire для подключения OneWire устройств.
DallasTemperature sensors(&oneWire);// Указание, что устройством oneWire является термодатчик от  Dallas Temperature.
//===================================================================================================//
void setup()
{
  lcd.init(); // инициализация LCD
  lcd.backlight();  // включаем подсветку
  lcd.clear();  // очистка дисплея
  lcd.setCursor(2, 0); // устанавливаем курсор,  4 знак 1 строка
  lcd.print("Platonova Larisa"); // вывод сообщения на LCD
  Serial.begin(9600);// Запуск СОМ порта.
  Serial.println("Start temperature measurement");
  sensors.begin();
} // Запуск сенсора.

//===================================================================================================//
void loop()
{
  Serial.println("Please wait..."); //печать в порт "Please wait..."
  sensors.requestTemperatures();  // команда опроса температуры
  Serial.print("Home=");  //печать в порт "Home"
  lcd.setCursor(0, 1);  // устанавливаем курсор,  1 знак 2 строка
  lcd.print("Home="); //печать на LCD "Home"
  lcd.print(sensors.getTempCByIndex(0));
  Serial.print(sensors.getTempCByIndex(0)); // Печать в порт температуры, "0" в данном случае указывает на первое устройство в шине
  delay(200);
  Serial.print("Work=");  //печать в порт "Work"
  lcd.setCursor(0, 2); //устанавливаем курсор,  1 знак 3 строка
  lcd.print("Work="); //печать на LCD "Work"
  lcd.print(sensors.getTempCByIndex(1));
  Serial.println(sensors.getTempCByIndex(1)); // печать в порт температуры, "1" в данном случае указывает на второе устройство в шине
  delay(1000);
  lcd.setCursor(0, 3); // устанавливаем курсор,  2 знак 4 строка
  lcd.print("RUN (Sec):"); //печать на LCD "RUN (Sec):"
  lcd.write(byte(380)); //
  lcd.write(byte(381)); //
  lcd.write(byte(382)); //
  lcd.write(byte(383)); //
  lcd.write(byte(384)); //
 // lcd.print(millis() / 1000); // вывод числа секунд после сброса на LCD

}
//===================================================================================================//
