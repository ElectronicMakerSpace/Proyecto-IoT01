# ATMEGA2560 + Sigfox

El proyecto tiene la finalidad de mostrar algunos ejemplos de la plataforma <b>Sigfox</b> integrada con el microcontrolador ATMEGA2560 y soportado con el [IDE](https://www.arduino.cc/en/main/software) de Arduino para una mejor implementación de los proyectos del "Internet de las Cosas".

## Instalación 

Para este caso no es necesaria otra herramienta mas que el IDE de Arduino, asegurese de tener instalados los [drivers](https://www.ftdichip.com/FTDrivers.htm) del chip FTDI para que se pueda lograr la comunicación de la PC con el KIT de Desarrollo.

## Hola Mundo con Sigfox 

Para verificar que nuestro KIT de Desarrollo funciona correctamente cargue el siguiente código donde, nos arrojara la dupla PAC/ID mediante el monitor serial de arduino y así logremos activarlo en la plataforma [Sigfox](https://buy.sigfox.com/activate).

```c++
const int boton=6;
char RespuestaSigfox[50];
char ID[51];
char PAC[51];

void setup() 
{
  Serial1.begin(9600);
  Serial.begin(9600);
  pinMode(boton, INPUT);
  pinMode(7, OUTPUT);
  Serial.println("\t\rInicializando Comunicacion Wisol\n");
}

void leer_info(int x, int y)
{
  digitalWrite(7, HIGH);
  delay(1000);
  enviarcomandoATSigfox("AT");
  enviarcomandoATSigfox("AT$I=10");
  strcpy(ID,RespuestaSigfox);
  enviarcomandoATSigfox("AT$I=11");
  strcpy(PAC,RespuestaSigfox);
  enviarcomandoATSigfox("AT$RC");
  enviarcomandoATSigfox("AT$SF=0102030405");
  digitalWrite(7, LOW);
  delay(500);
  Serial1.print("\tID: ");
  Serial1.println(ID);
  Serial1.print("\tPAC: ");
  Serial1.println(PAC);
  
  Serial.print("\tID: ");
  Serial.println(ID);
  Serial.print("\tPAC: ");
  Serial.println(PAC);
  Serial.print("\n\n");
}

void loop() 
{
  if (digitalRead(boton)==LOW)
  {
    int primerNumero=random(1,100);       //Declaro e inicializo una variable global
    int segundoNumero=random(1,100);
    leer_info(primerNumero,segundoNumero);
    delay(2000);
  }
}

void enviarcomandoATSigfox(char* comandoAT)
{
  unsigned long x=0;
  Serial.print("Comando Sigfox-->");
  Serial.println(comandoAT);
  while( Serial1.available() > 0) Serial1.read();
    x = 0;
  memset(RespuestaSigfox, '\0',sizeof(RespuestaSigfox)); 
  Serial1.print(comandoAT);
  Serial1.print("\r\n");
  while(true)
  {
    if(Serial1.available() != 0)
    {   
      RespuestaSigfox[x] = Serial1.read();
      x++;
      if (strstr(RespuestaSigfox, "\n") != NULL)
      {
        Serial.print("Respuesta: \r");
        Serial.println(RespuestaSigfox);
        break;
      }
    }
  }
}
```
## Pagina para comprar tu KIT de Desarrollo Sigfox con ATMEGA2560
[Puedes Adquirir tu Kit en AG Electrónica](https://agelectronica.com/?merca=IOT,SIGFOX)

## License
[MIT](https://choosealicense.com/licenses/mit/)
