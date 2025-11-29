# REPORTE-5-
## Descripción
Se implementará lo aprendido usando una ```LCD 16x2 I2C``` para mostrar datos obtenidos de un ultrasonico y a su ves será visible una combinación de leds de acuerdo a los valores del mismo ultrasonico por medio del simulador Wokwi.
(https://wokwi.com/projects/new/esp32)
## Materiales
1. Simulador WOKWI, dentro del simulador se hará uso de los siguientes componentes:
- Cableado
-```ESP32```
- ```LCD 16x2 I2C```
- ```HC-SR04 ULTRASONIC DISTANCE SENSOR```
- 4 leds
- 4 resistencias
- Libreria ```LiquidCrystal I2C```
## Instrucciones
1. Abrir el simulador WOKWI en el navegador ![](https://wokwi.com/projects/new/esp32)

![](https://github.com/EfrenQA/Reporte-1/blob/main/wokwi.png?raw=true)

3. Elegir la tarjeta ```ESP32```

 ![](https://github.com/EfrenQA/Reporte-1/blob/main/ESP32.png?raw=true)

4. Selección del sketch

![](https://github.com/EfrenQA/Reporte-1/blob/main/ESP32-2.png?raw=true)

5. Al seleccionar el skecth se mostrara una interfas como se muestra en la imagen

![](https://github.com/EfrenQA/Reporte-1/blob/main/INTERFAS.png?raw=true)

6. Se hará la selección de la libreria ```LCD 16x2 I2C``` para el LCD, para esto se tiene que dirigir Library Manager

![](https://github.com/EfrenQA/Reporte-1/blob/main/AGREGAR%20LIBRERIA.png?raw=true)

7. Una ves en la pestaña de librerias, se tiene que clickear sobre el botón de "+", seguido de esto se abrira un cuadro de busqueda donde se buscara la libreria "LiquidCrystal I2C".

![](https://github.com/EfrenQA/REPORTE-2/blob/main/libreria%20liquid%20crystal.png?raw=true)

8. Para colocar el LCD y ultrasonico en el espacio de simulación se tiene que clickear en el botón de "+" que esta en el cuadro de simuación.

![](https://github.com/EfrenQA/Reporte-1/blob/main/AGREGAR%20COMPONENTES.png?raw=true)

9. Para la selección del LCD, ultrasonico, LEDS y resistencias, podemos escribir su nombre en la barra de buscador o desplazarnos en las opciones.

![](https://github.com/EfrenQA/Reporte-3/blob/main/selecci%C3%B3n%20de%20ultrasonico%20.png?raw=true)

![](https://github.com/EfrenQA/REPORTE-2/blob/main/sleccion%20de%20lcd.png?raw=true)

![](https://github.com/EfrenQA/REPORTE-5-/blob/main/LED%20y%20resistencias.png?raw=true)


10. Una vez que se tengan los once componentes en el cuadro de simulación, se procede a la conección entre la tarjeta y los dos componentes de la siguiente forma:
- PIN GND del ultrasonico al PIN GND de la tarjeta.
- PIN VCC del ultrasonico al PIN VCC de la tarjeta.
- PIN TRIG del ultrasonico al PIN 4 de la tarjeta.
- PIN ECHO del ultrasonico al PIN 15 de la tarjeta.

- PIN GND del LCD al PIN GND de la tarjeta.
- PIN VCC del LCD al PIN VCC de la tarjeta.
- PIN SDA del LCD al PIN 21 de la tarjeta.
- PIN SCL del LCD al PIN 22 de la tarjeta.

- LED 1 pin corto al pin 19 de la tarjeta. 
- LED 2 pin corto al pin 18 de la tarjeta.
- LED 3 pin corto al pin 5 de la tarjeta.
- LED 4 pin corto al pin 17 de la tarjeta.

- Todas las resistencias a pin largo del LED y su otro extremo al GND de la tarjeta
![](https://github.com/EfrenQA/REPORTE-5-/blob/main/LAYOUT%205.png?raw=true)


10. En el lado de "SKETCH" se debe colocar el siguiente código:
```
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4
const int Trigger = 4;   //Pin digital 2 para el Trigger del sensor
const int Echo = 15;   //Pin digital 3 para el Echo del sensor
const int led1 = 19;
const int led2 = 18;
const int led3 = 5;
const int led4 = 17;

// defines variables
long duration;
int distance;
int safetyDistance;
LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
  Serial.begin(9600);//iniciailzamos la comunicación
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
  lcd.init();
  lcd.backlight();
pinMode(led1, OUTPUT);
pinMode(led2, OUTPUT);
pinMode(led3, OUTPUT);
pinMode(led4, OUTPUT);
}

void loop()
{

// Clears the trigPin
digitalWrite(Trigger, LOW);
delayMicroseconds(2);

// Sets the trigPin on HIGH state for 10 micro seconds
digitalWrite(Trigger, HIGH);
delayMicroseconds(10);
digitalWrite(Trigger, LOW);

// Reads the echoPin, returns the sound wave travel time in microseconds
duration = pulseIn(Echo, HIGH);

// Calculating the distance
distance= duration*0.034/2;
safetyDistance = distance;

if (safetyDistance >= 300 && safetyDistance <=400)
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
     lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("BIENVENIDOS");
  lcd.setCursor(0, 1);
  lcd.print("MODULO 5");

delay(1500);
 lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("EFREN QUIROZ");
  lcd.setCursor(0, 1);
  lcd.print("ING.INDUSTRIAL");
 
delay(1500);
lcd.clear();
lcd.setCursor(0, 0);
  lcd.print("Distancia: ");
  lcd.print(distance);      //Enviamos serialmente el valor de la distancia
  lcd.print("cm");
lcd.setCursor(0, 1);
lcd.print("tanque a -1/4");
  delay(1000);          //Hacemos una pausa de 100ms
}
else if(safetyDistance >=200 && safetyDistance<=299) 
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
       lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("BIENVENIDOS");
  lcd.setCursor(0, 1);
  lcd.print("MODULO 5");

delay(1500);
 lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("EFREN QUIROZ");
  lcd.setCursor(0, 1);
  lcd.print("ING.INDUSTRIAL");
 
delay(1500);
lcd.clear();
lcd.setCursor(0, 0);
  lcd.print("Distancia: ");
  lcd.print(distance);      //Enviamos serialmente el valor de la distancia
  lcd.print("cm");
lcd.setCursor(0, 1);
lcd.print("tanque a 1/2");
  delay(1000);          //Hacemos una pausa de 100ms
}
else if(safetyDistance >=100 && safetyDistance <=199) 
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, HIGH);
  digitalWrite(led4, LOW);
       lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("BIENVENIDOS");
  lcd.setCursor(0, 1);
  lcd.print("MODULO 5");

delay(1500);
 lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("EFREN QUIROZ");
  lcd.setCursor(0, 1);
  lcd.print("ING.INDUSTRIAL");
 
delay(1500);
lcd.clear();
lcd.setCursor(0, 0);
  lcd.print("Distancia: ");
  lcd.print(distance);      //Enviamos serialmente el valor de la distancia
  lcd.print("cm");
lcd.setCursor(0, 1);
lcd.print("tanque a 3/4");
  delay(1000);          //Hacemos una pausa de 100ms
}
else
{
 digitalWrite(led1, HIGH);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, HIGH);
  digitalWrite(led4, HIGH);
       lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("BIENVENIDOS");
  lcd.setCursor(0, 1);
  lcd.print("MODULO 5");

delay(1500);
 lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("EFREN QUIROZ");
  lcd.setCursor(0, 1);
  lcd.print("ING.INDUSTRIAL");
 
delay(1500);
lcd.clear();
lcd.setCursor(0, 0);
  lcd.print("Distancia: ");
  lcd.print(distance);      //Enviamos serialmente el valor de la distancia
  lcd.print("cm");
lcd.setCursor(0, 1);
lcd.print("tanque lleno");
  delay(1000);          //Hacemos una pausa de 100ms
}
// Prints the distance on the Serial Monitor
Serial.print("Distance: ");
Serial.println(distance);
delay (2000);
}
```
11. como último paso se inicia el simulador con el botón de "play" y se podra visualizar las lecturas del ultrsonico así como las combinaciones de leds.

## Instrucciones de operación
1. Iniciar simulación con el botón de "PLAY"
2. Visualizar los datos que se mostrarán en LCD
- Cuando el ultrsonico este en el rango de 0 - 99, se prendarán los 4 leds y el LCD marcará "Tanque lleno"
![](https://github.com/EfrenQA/REPORTE-5-/blob/main/TANQUE%20LLENO.png?raw=true)

- Cuando el ultrsonico este en el rango de 100 - 199, se prendarán 3 leds y el LCD marcará "Tanque a 3/4"
![](https://github.com/EfrenQA/REPORTE-5-/blob/main/TANQUE%2075%25.png?raw=true)

- Cuando el ultrsonico este en el rango de 200 - 299, se prendarán los 2 leds y el LCD marcará "Tanque a 1/2"
![](https://github.com/EfrenQA/REPORTE-5-/blob/main/TANQUE%2050%25.png?raw=true)

- Cuando el ultrsonico este en el rango de 300 - 400, se prendarán 1 led y el LCD marcará "Tanque a -1/4" para indicar que el tanque puede estar a menos de 1/4 de capacidad.


![](https://github.com/EfrenQA/REPORTE-5-/blob/main/TANQUE%2025%25.png?raw=true)

## Resultados
Al finalizar cada paso se podrá obtener datos desde la simulación del LCD
![](https://github.com/EfrenQA/REPORTE-5-/blob/main/TANQUE%20LLENO.png?raw=true)
![](https://github.com/EfrenQA/REPORTE-5-/blob/main/TANQUE%2075%25.png?raw=true)
![](https://github.com/EfrenQA/REPORTE-5-/blob/main/TANQUE%2050%25.png?raw=true)
![](https://github.com/EfrenQA/REPORTE-5-/blob/main/TANQUE%2025%25.png?raw=true)

## Créditos
Autor Ing. Efren David Quiroz Ayala 
