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
