#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_BMP085.h>
#include <DHT.h>
#include <MPU6050.h>
#include <SoftwareSerial.h>

// Sensör ve Modül Ayarları
#define DHTPIN 7
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

MPU6050 imu;
Adafruit_BMP085 bmp;
SoftwareSerial gpsSerial(18, 19); // GPS RX (18), TX (19)

void setup() {
  Serial.begin(9600);
  Wire.begin();

  // IMU Başlat
  imu.initialize();
  if (!imu.testConnection()) {
    Serial.println("IMU Bağlantı Başarısız!");
    while (1);
  }

  // Barometre Başlat
  if (!bmp.begin()) {
    Serial.println("BMP180 Algılanamadı!");
    while (1);
  }

  // DHT Başlat
  dht.begin();

  // GPS Başlat
  gpsSerial.begin(9600);

  Serial.println("Sistem Başlatıldı!");
}

void loop() {
  // IMU Verileri
  int16_t ax, ay, az, gx, gy, gz;
  imu.getMotion6(&ax, &ay, &az, &gx, &gy, &gz);
  Serial.print("IMU - Ax: "); Serial.print(ax);
  Serial.print(", Ay: "); Serial.print(ay);
  Serial.print(", Az: "); Serial.println(az);

  // Barometre Verileri
  float temperature = bmp.readTemperature();
  float pressure = bmp.readPressure();
  float altitude = bmp.readAltitude();
  Serial.print("BMP180 - Sıcaklık: "); Serial.print(temperature);
  Serial.print(" C, Basınç: "); Serial.print(pressure);
  Serial.print(" Pa, İrtifa: "); Serial.println(altitude);

  // DHT11 Verileri
  float temp = dht.readTemperature();
  float hum = dht.readHumidity();
  Serial.print("DHT11 - Sıcaklık: "); Serial.print(temp);
  Serial.print(" C, Nem: "); Serial.println(hum);

  // GPS Verileri
  while (gpsSerial.available() > 0) {
    char c = gpsSerial.read();
    Serial.print(c);
  }

  delay(1000);
}
