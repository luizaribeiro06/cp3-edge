# cp3-edge
![image](https://github.com/user-attachments/assets/0e836a2d-bd40-4c76-9a91-cc4435e301ea)

#Grupo:
- Luiza Ribeiro rm: 560200
- Gabrielly Candido rm: 560916

#Código
#include <LiquidCrystal_I2C.h> 
LiquidCrystal_I2C lcd(0x27, 16, 2);  

#include <DHT.h>
#define DHTPIN 2                  
#define DHTTYPE DHT22
DHT dht(DHTPIN, DHTTYPE);          

int ledR = 13;
int ledY = 12;
int ledG = 11;
int trigger = 7;
int echo = 8;
long dist = 0;  

void setup() {
  pinMode(ledR, OUTPUT);
  pinMode(ledY, OUTPUT);
  pinMode(ledG, OUTPUT);
  pinMode(trigger, OUTPUT);
  pinMode(echo, INPUT);

  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Bem vindo a");
  lcd.setCursor(0, 1);
  lcd.print("Vinheria Agnello!");

  dht.begin();
  delay(4000);  
  Serial.begin(9600);
  lcd.clear(); 
  delay(4000);
}

void loop() {
  lcd.clear();
  float temperatura = dht.readTemperature();
  float umidade = dht.readHumidity();     

  lcd.setCursor(0, 0);
  lcd.print("Temp: " + String(temperatura, 1) + "C");
  lcd.setCursor(0, 1);
  lcd.print("Umidade: " + String(umidade, 1) + "%");
  delay(2000);
  lcd.clear();  

  if (umidade > 40) {
    lcd.setCursor(0, 0);
    lcd.print("Umidade alta!");
  } else if (umidade < 60) {
    lcd.setCursor(0, 0);
    lcd.print("Umidade baixa!");
  } else {
    lcd.setCursor(0, 0);
    lcd.print("Umidade ideal!");
  }

  lcd.setCursor(0, 1);
  lcd.print("Umidade: " + String(umidade, 1) + "%");
  delay(3000);

  lcd.clear();
  if (temperatura > 15) {
    lcd.setCursor(0, 0);
    lcd.print("Temp alta!");
  } else if (temperatura < 12) {
    lcd.setCursor(0, 0);
    lcd.print("Temp baixa!");
  } else {
    lcd.setCursor(0, 0);
    lcd.print("Temp ideal!");
  }
  lcd.setCursor(0, 1);
  lcd.print("Temp: " + String(temperatura, 1) + "C");
  delay(3000);

  digitalWrite(trigger, LOW);
  delayMicroseconds(5);
  digitalWrite(trigger, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigger, LOW);

  dist = pulseIn(echo, HIGH);  
  dist = dist / 58;            

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Dist: " + String(dist) + " cm");

  if (dist < 40) {
    lcd.setCursor(0, 1);
    lcd.print("Estoque cheio");
    digitalWrite(ledR, LOW);  
    digitalWrite(ledY, LOW);   
    digitalWrite(ledG, HIGH);  
  } else if (dist >= 40 && dist < 70) {
    lcd.setCursor(0, 1);
    lcd.print("Estoque médio");
    digitalWrite(ledR, LOW);  
    digitalWrite(ledY, HIGH);  
    digitalWrite(ledG, LOW);   
  } else {
    lcd.setCursor(0, 1);
    lcd.print("Estoque pouco");
    digitalWrite(ledR, HIGH); 
    digitalWrite(ledY, LOW);  
    digitalWrite(ledG, LOW);  
  }

  delay(3000);  
}
