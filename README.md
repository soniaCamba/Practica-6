# Practica-6

# Practica 6A - Lectura/escritura de memoria sd

## CODIGO
```
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>

File myFile;

void setup()
{
    Serial.begin(115200);
    Serial.print("Iniciando SD ...");
    SPI.begin(18,19,23,5);

    if (!SD.begin(5)) {
        Serial.println("No se pudo inicializar");
        return;
    }
    Serial.println("inicializacion exitosa");
    
    if(!SD.exists("/archivo.txt")){
            Serial.println("example.txt exists.");
    }

    else{
            Serial.println("example.txt no exists.");
    }

    myFile = SD.open("/archivo.txt", FILE_WRITE);
    myFile.close();

    if (SD.exists("/archivo.txt")){


            Serial.println("archivo.txt exists. ");
    }
    else{
            Serial.println("archivo.txt doesn't exist");
    }
    myFile = SD.open("/archivo.txt", FILE_WRITE);//abrimos  el archivo 
    myFile.println("Hola mundo");
    myFile.close();
    myFile=SD.open("/archivo.txt");
    if (myFile) {
        Serial.println("archivo.txt:");
        while (myFile.available()) {
            Serial.write(myFile.read());
        }
        myFile.close(); //cerramos el archivo
    } else {
        Serial.println("Error al abrir el archivo");
    }
}

void loop()
{}
```

## FUNCIONAMIENTO

> Declarammos las librerias necesarias y inicializamos la variable miFile
```
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>

File myFile;
```
> Dentro del setup iniciamos la sd, si se ha podido o no lo sacamos por pantalla para informar al usuario
```
Serial.begin(115200);
Serial.print("Iniciando SD ...");
SPI.begin(18,19,23,5);

if (!SD.begin(5)) {
     Serial.println("No se pudo inicializar");
     return;
 }
Serial.println("inicializacion exitosa");
```
> A continuación examinamos si dentro de la targeta hay algun archivo de texto
```
if(!SD.exists("/archivo.txt")){
  Serial.println("example.txt exists.");
}

else{
   Serial.println("example.txt no exists.");
}
```
> Seguidamente generamos un nuvo archivo
```
myFile = SD.open("/archivo.txt", FILE_WRITE);
myFile.close();
```
> Comprobamos si se ha creado
```
if (SD.exists("/archivo.txt")){
  Serial.println("archivo.txt exists. ");
 }
else{
  Serial.println("archivo.txt doesn't exist");
}
```
> Abrimos el archivo
```
myFile = SD.open("/archivo.txt", FILE_WRITE);
```
> Escribimos en el 
```
myFile.println("Hola mundo");
```
> Lo cerramos
```
myFile.close();
```
Y finalmente lo abrimos de nuevo para leerlo y si no podemos lo mencionamos en pantalla
```
myFile=SD.open("/archivo.txt");
if (myFile) {
  Serial.println("archivo.txt:");
  while (myFile.available()) {
       Serial.write(myFile.read());
  }
  myFile.close(); //cerramos el archivo
} 
else {
  Serial.println("Error al abrir el archivo");
}
```

# Practica 6B - Lectura de etiqueta RFID

## CODIGO
```
#include <SPI.h>
#include <MFRC522.h>

#define RST_PIN	21    //Pin 9 para el reset del RC522
#define SS_PIN	15   //Pin 10 para el SS (SDA) del RC522
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522

void setup() {
	Serial.begin(115200); //Iniciamos la comunicación  serial
	SPI.begin(14,12,13,15);        //Iniciamos el Bus SPI
	mfrc522.PCD_Init(); // Iniciamos  el MFRC522
	Serial.println("Lectura del UID");
}

void loop() {
	// Revisamos si hay nuevas tarjetas  presentes
	if ( mfrc522.PICC_IsNewCardPresent()) 
        {  
  		//Seleccionamos una tarjeta
            if ( mfrc522.PICC_ReadCardSerial()) 
            {
                  // Enviamos serialemente su UID
                  Serial.print("Card UID:");
                  for (byte i = 0; i < mfrc522.uid.size; i++) {
                          Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
                          Serial.print(mfrc522.uid.uidByte[i], HEX);   
                  } 
                  Serial.println();
                  // Terminamos la lectura de la tarjeta  actual
                  mfrc522.PICC_HaltA();         
            }      
	}	
}
```

## DESCRIBIR LA SALIDA POR EL PUERTO SERIE:

Una vez ya le hayamos dado a "Upload and Monitor" nos saldrá por pantalla "Lectura del UID". Y cada vez que pasemos una targeta saldrá "Card UID:" junto al VID de la targeta.

## FUNCIONAMIENTO

> Añadimos las librerias necesarias
```
#include <SPI.h>
#include <MFRC522.h>
```
> Inicializamos los pines que vamos a usar y creamos el objeto para el RC522
```
#define RST_PIN	17    //Pin 9 para el reset del RC522
#define SS_PIN	4   //Pin 10 para el SS (SDA) del RC522
MFRC522 mfrc522(SS_PIN, RST_PIN); 
```
> En el setup:
> 1.Iniciamos la comunicación serial
> 2.Iniciamos el Bus SPI
> 3.Iniciamos  el MFRC522
> 4.Y sacamos por pantalla "Lectura del UID"
```
void setup() {
	Serial.begin(115200);
	SPI.begin(14,12,13,15);
	mfrc522.PCD_Init();
	Serial.println("Lectura del UID");
}
```
> En el loop:
> Revisamos si hay nuevas tarjetas  presentes
```
void loop() {
	if ( mfrc522.PICC_IsNewCardPresent()) 
    {
```
> Seleccionamos una tarjeta
```
if ( mfrc522.PICC_ReadCardSerial()) 
{
```
> Enviamos serialemente su UID
```
Serial.print("Card UID:");
for (byte i = 0; i < mfrc522.uid.size; i++) {
  Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
  Serial.print(mfrc522.uid.uidByte[i], HEX);   
} 
Serial.println();
```
> Terminamos la lectura de la tarjeta  actual
```
mfrc522.PICC_HaltA();  
```
> Y por ultimo cerramos todos los corchetes que tengamos abiertos.
