# spd_parcial
Tp Arduino. Parcial de SPD parte 1.

// C++ code
/*CONNOR FORNICA
Ejercicio 3-2
Con 2 display 7 segmentos generar un contador que comience
en el valor 0 llegue hasta 99*/

#define G 13
#define F 12
#define A 11
#define B 10
#define C 5
#define D 6
#define E 7

#define SUBE 4
#define BAJA 3
#define RESET 2

#define UNIDAD A3
#define DECENA A5
#define APAGADOS 0
#define Tiempo 200

int contadorDigito = 0;
int unidad = 0;
int decena = 0;
int subir = 1;
int subirPrevia = 1;
int bajar = 1;
int bajarPrevia = 1;
int reset = 1;
int resetPrevia = 1;

void setup()
{
  pinMode(A, OUTPUT);
  pinMode(B, OUTPUT);
  pinMode(C, OUTPUT);
  pinMode(D, OUTPUT);
  pinMode(E, OUTPUT);
  pinMode(F, OUTPUT);
  pinMode(G, OUTPUT);
  
  pinMode(UNIDAD, OUTPUT);
  digitalWrite(UNIDAD, LOW); // Establece el pin UNIDAD en estado bajo
  pinMode(DECENA, OUTPUT);
  digitalWrite(DECENA, LOW); // Establece el pin DECENA en estado bajo
  encenderDigito(0);

  pinMode(SUBE, INPUT_PULLUP);
  pinMode(BAJA, INPUT_PULLUP);
  pinMode(RESET, INPUT_PULLUP);
  
  Serial.begin(9600);
}
void loop()
{
  int presionado = botonpresionado();
  Serial.println(presionado);
  if(presionado == SUBE)
  {
    contadorDigito++;
    if(contadorDigito > 99)
      contadorDigito = 0;
  }
  
  else 
    if(presionado == BAJA)
  {
    contadorDigito--;
    if(contadorDigito < 0)
      contadorDigito = 99;
  }
  
  else 
    if(presionado == RESET)
  {	
    contadorDigito = 0;
  }
}


int botonpresionado(void)
{
    subir = digitalRead(SUBE);
    bajar = digitalRead(BAJA);
    reset = digitalRead(RESET);
  	
    if (subir)
      subirPrevia = 1;
    if (bajar)
      bajarPrevia = 1;
    if (reset)
      resetPrevia = 1;
  

    if (subir == 0 && subirPrevia != 1)
    {
      subirPrevia = 0;
      return SUBE;
    }

    if (bajar == 0 && bajarPrevia != 1)
    {
      bajarPrevia = 0;
      return BAJA;
    }

    if (reset == 0 && resetPrevia != 1)
    {
      resetPrevia = 0;
      return RESET;
    }
  
return 0;
  
}

void encenderCuenta()
{
  encenderDigitos(APAGADOS);
  encenderDigito(contadorDigito / 10);
  encenderDigitos(DECENA);
  encenderDigitos(APAGADOS);
  delay(400);
  encenderDigito(contadorDigito - 10*(contadorDigito/10));
  encenderDigitos(UNIDAD);
}

void encenderDigito(int digito)
{ 
  digitalWrite(A, LOW);
  digitalWrite(B, LOW);
  digitalWrite(C, LOW);
  digitalWrite(D, LOW);
  digitalWrite(E, LOW);
  digitalWrite(F, LOW);
  digitalWrite(G, LOW);
  
  switch (digito) 
  {
    case 0:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      break;
    case 1:
      digitalWrite(A, HIGH);
      digitalWrite(D, HIGH);
      break;
    case 2:
      digitalWrite(B, HIGH);
      digitalWrite(G, HIGH);
      break;
    case 3:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      break;
    case 4:
      digitalWrite(C, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(G, HIGH);
      break;
    case 5:
      digitalWrite(A, HIGH);
      digitalWrite(C, HIGH);
      break;
    case 6:
      digitalWrite(A, HIGH);
      break;
    case 7:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(G, HIGH);
      break;
    case 8:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    case 9:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(G, HIGH);
      break;
  }
}

void encenderDigitos(int digito)
{
  if(digito == UNIDAD)
  {
    digitalWrite(UNIDAD, LOW);
  	digitalWrite(DECENA, HIGH);
    delay(Tiempo);
  }
  
  else 
    if(digito == DECENA)
  {
    digitalWrite(UNIDAD, HIGH);
	digitalWrite(DECENA, LOW);
    delay(Tiempo);
  }
  else
  {
    digitalWrite(UNIDAD, HIGH);
   	digitalWrite(DECENA, HIGH);
  }
}

