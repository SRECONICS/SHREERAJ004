- 👋 Hi, I’m @SHREERAJ004
- 👀 I’m interested in modifying C program 
- 🌱 I’m currently learning Embedded system in C

This is C++ code sketch designed to run on an ESP8266 microcontroller, like a NodeMCU or Wemos D1 Mini, to display an analog clock face on a SSD1306 OLED display. It fetches the current time via NTP (Network Time Protocol).
Since you're posting this to GitHub, here is a breakdown of the code's functionality, its libraries, and the hardware it targets:
📜 Code Explanation
1. Includes and Definitions
The code starts by including necessary libraries:
 * <ESP8266WiFi.h>: Handles the Wi-Fi connection for the ESP8266.
 * <time.h>: Provides functions for time and date management (like time() and localtime()).
 * <SPI.h>, <Wire.h>: Standard libraries for communication protocols, specifically I2C (Wire) for the OLED.
 * <Adafruit_GFX.h>, <Adafruit_SSD1306.h>: Libraries for graphics and controlling the SSD1306 OLED display.
 * #define OLED_RESET LED_BUILTIN: Defines the reset pin for the OLED.
 * Adafruit_SSD1306 display(OLED_RESET);: Initializes the display object (assumes an I2C connection and a 128x32 pixel display based on the error check).
 * const char* ssid = "SRECONICS_04";: Your Wi-Fi network name.
 * const char* password = "sreesree";: Your Wi-Fi password.
 * int timezone = 7 * 3600;: Sets the timezone offset in seconds (7 hours).
 * int dst = 0;: Sets Daylight Saving Time offset to 0 (off).
2. setup() Function
This section initializes the hardware and connects to the internet.
 * OLED and LED Setup:
   * display.begin(SSD1306_SWITCHCAPVCC, 0x3C);: Starts the OLED display, using the internal charge pump and the common I2C address 0x3C.
   * pinMode(ledPin, OUTPUT); digitalWrite(ledPin, LOW);: Initializes a built-in LED (pin 13 on some boards) and turns it off.
   * Serial.begin(9600);: Initializes serial communication for debugging.
 * Wi-Fi Connection:
   * The code prints status messages to the OLED (display.println("Wifi connecting to... ")) and then attempts to connect using WiFi.begin(ssid, password);.
   * A while loop with delay(500) waits for the connection and prints dots (.) to the screen.
 * NTP Time Sync:
   * configTime(timezone, dst, "pool.ntp.org", "time.nist.gov");: Configures the ESP8266 to use Network Time Protocol (NTP) to fetch the time from the specified servers, adjusting for your timezone.
   * A while loop waits until the ESP8266 receives a valid time (while(!time(nullptr))), printing status to the OLED and Serial Monitor.
3. loop() Function
This is the main loop that continuously updates and draws the analog clock.
 * Time Retrieval:
   * time_t now = time(nullptr);: Gets the current time as a timestamp.
   * struct tm* p_tm = localtime(&now);: Converts the timestamp into a human-readable structure (tm), containing hour, minute, second, day, etc.
 * Clock Face Drawing:
   * display.drawCircle(...): Draws a small dot at the center of the display (64, 32) as the center point of the hands.
   * The hour ticks are drawn using a for loop that iterates from 0° to 360° in 30° increments. It calculates the start and end points of a small line using trigonometry (sine and cosine) and draws the tick mark.
 * Hand Drawing (Trigonometry):
   * The code calculates the angle for each hand, converts it to radians (/ 57.29577951), and uses trigonometry to find the endpoint (x3, y3) of the line representing the hand.
   * Second Hand: Angle is tm_sec * 6 (6 degrees per second). Full radius (r).
   * Minute Hand: Angle is tm_min * 6 (6 degrees per minute). Slightly shorter radius (r-3).
   * Hour Hand: Angle is tm_hour * 30 + int((p_tm->tm_min / 12) * 6 ). The (tm_min / 12) * 6 term makes the hour hand move smoothly between hour marks. Shorter radius (r-11).
   * display.drawLine(64, 32, x3, y3, WHITE);: Draws the hand from the center to the calculated endpoint.
 * Date Display:
   * display.print(p_tm->tm_mday);: Prints the current day of the month near the center.
 * Update and Clear:
   * display.display();: Pushes the buffered pixel data to the physical OLED screen.
   * delay(100);: Waits 100 milliseconds.
   * display.clearDisplay();: Clears the frame buffer, ensuring the hands are erased before the next drawing cycle.
🛠️ Hardware & Library Requirements
To make this code work, you'll need:
| Component | Purpose | Notes |
|---|---|---|
| Microcontroller | ESP8266 (e.g., NodeMCU, ESP-01) | Required for Wi-Fi and running the sketch. |
| Display | SSD1306 128x32 I2C OLED | The code specifically checks for a height of 32 pixels. |
| Connection | I2C (SDA and SCL pins) | Connect the display to the ESP8266's SDA/SCL pins. |
Libraries to Install
You must install these libraries in the Arduino IDE's Library Manager:
 * ESP8266 Boards: Install the ESP8266 core for Arduino (via the Boards Manager).
 * Adafruit GFX Library
 * Adafruit SSD1306
 * 
- 📫 How to reach me https://www.linkedin.com/in/shreeraj-s-760167241

<!---
SHREERAJ004/SHREERAJ004 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
