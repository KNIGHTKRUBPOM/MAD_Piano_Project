# 🎹 Project Piano - Hardware Pinout (V6.1 Standalone - Matrix Edition)

เอกสารระบุการเชื่อมต่อขาสัญญาณทั้งหมดระหว่าง **STM32F767ZIT6 (Nucleo-144)** และอุปกรณ์ต่อพ่วงต่างๆ สำหรับระบบ Standalone Piano Engine

---

## 📺 1. TFT Display & Touch (ILI9341 SPI)
ใช้ **Hardware SPI1** ในการสื่อสารข้อมูลภาพและสัมผัส โดยใช้ขาฝั่ง **Outside (แถวนอก)** ของ Morpho Header เป็นหลัก

| Function | Screen Pin | STM32 Pin | Header Pin | Port Mode | Note |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **VCC** | VCC | 3.3V | CN7-14 (In) | Power | จ่ายไฟ 3.3V |
| **GND** | GND | GND | CN7-8 (In) | Ground | สายกราวด์ |
| **CS** | CS | **PD14** | **CN7-16 (Out)** | Output | Chip Select (Screen) |
| **RESET** | RESET | **PF12** | **CN7-20 (Out)** | Output | Hardware Reset (Active Low) |
| **DC/RS** | DC/RS | **PD15** | **CN7-18 (Out)** | Output | Data / Command Selection |
| **SDI (MOSI)**| SDI | **PD7** | **CN9-2 (Out)** | AF5 | SPI1 Master Out Slave In |
| **SCK** | SCK | **PA5** | **CN7-10 (Out)** | AF5 | SPI1 Clock Signal |
| **LED** | LED | **PB1** | **CN10-24 (Out)**| Output | Backlight Control (High = On) |
| **SDO (MISO)**| SDO | **PA6** | **CN7-12 (Out)** | AF5 | SPI1 Master In Slave Out |
| **T_CS** | T_CS | **PF13** | **CN7-17 (In)** | Output | Chip Select (Touch) |
| **T_IRQ** | T_IRQ | **PF14** | **CN7-15 (In)** | Input | Touch Interrupt Signal |

---

## 🔊 2. Audio DAC (PCM5102A I2S)
ใช้ **I2S2 (SPI2 Interface)** สำหรับส่งสัญญาณเสียงคุณภาพสูง (48kHz/16-bit Stereo)

| DAC Pin | Function | STM32 Pin | Header Pin | Port Mode | Note |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **VCC** | Power | 5V / 3.3V | CN10-6 / 11 | Power | แนะนำให้ใช้ 5V เพื่อกำลังขับที่ดี |
| **GND** | Ground | GND | CN10-22 (In) | Ground | สายกราวด์ |
| **BCK** | Bit Clock | **PB10** | **CN10-32 (Out)**| I2S2_CK | สัญญาณนาฬิกาบิต |
| **DIN** | Data In | **PC3** | **CN9-5 (In)** | I2S2_SD | ข้อมูลสัญญาณเสียง |
| **LCK (WS)** | Word Select| **PB12** | **CN7-7 (In)** | I2S2_WS | สัญญาณเลือกช่องเสียง (L/R) |

---

## 🎹 3. Piano Key Matrix (8x8 Matrix - Inside Pins Only)
เชื่อมต่อกับ AKAI MPK mini MK3 Keybed ผ่านขาฝั่ง **Inside (แถวใน)** ของ Morpho Header (ขาเลขคี่) เพื่อให้ตรงกับ Screw Terminal บนบอร์ดเขียว

### 🚥 Rows (Scan Outputs - Driven LOW)
| Row Index | STM32 Pin | Header Pin | Function | Note |
| :--- | :--- | :--- | :--- | :--- |
| **Row 0** | **PC6** | **CN7-1 (In)** | ROW 0 | S1 (First Contact Point) |
| **Row 1** | **PB15** | **CN7-3 (In)** | ROW 1 | S2 (Trigger Point for Velocity) |
| **Row 2** | **PB13** | **CN7-5 (In)** | ROW 2 | S1 |
| **Row 3** | **PA15** | **CN7-9 (In)** | ROW 3 | S2 |
| **Row 4** | **PC7** | **CN7-11 (In)** | ROW 4 | S1 |
| **Row 5** | **PB5** | **CN7-13 (In)** | ROW 5 | S2 |
| **Row 6** | **PB3** | **CN7-15 (In)** | ROW 6 | S1 |
| **Row 7** | **PA4** | **CN7-17 (In)** | ROW 7 | S2 |

### 🚦 Columns (Input Sensors - Pull-up enabled)
| Col Index | STM32 Pin | Header Pin | Function | Note |
| :--- | :--- | :--- | :--- | :--- |
| **Col 0** | **PB4** | **CN7-19 (In)** | COL 0 | อ่านสถานะปุ่ม (Active LOW) |
| **Col 1** | **PC2** | **CN10-9 (In)** | COL 1 | |
| **Col 2** | **PF4** | **CN10-11 (In)** | COL 2 | |
| **Col 3** | **PB6** | **CN10-13 (In)** | COL 3 | |
| **Col 4** | **PB2** | **CN10-15 (In)** | COL 4 | |
| **Col 5** | **PD13** | **CN10-19 (In)** | COL 5 | |
| **Col 6** | **PD12** | **CN10-21 (In)** | COL 6 | |
| **Col 7** | **PD11** | **CN10-23 (In)** | COL 7 | |

---

## 🎛️ 4. Analog Controls & UI Buttons
ส่วนควบคุมเสริมและการตั้งค่า

| Control | STM32 Pin | Header Pin | Function | Note |
| :--- | :--- | :--- | :--- | :--- |
| **Master VR** | **PA3 (A1)** | **CN9-1 (In)** | ADC1_IN3 | โวลุ่มปรับระดับเสียงหลัก (0-3.3V) |
| **User Button**| **PC13** | Onboard | Input | ปุ่มสีน้ำเงินบนบอร์ด (ใช้ Calibrate) |
| **Reset** | NRST | CN7-4 | Reset | ปุ่ม Reset สีดำบนบอร์ด |

---

## 💻 5. Debug Console (UART)
สำหรับการดู Log ผ่าน Computer (Serial Monitor)

| Function | STM32 Pin | Baud Rate | Note |
| :--- | :--- | :--- | :--- |
| **Debug TX** | **PD8** | 115200 | ผ่าน USB ST-Link (Virtual COM Port) |
| **Debug RX** | **PD9** | 115200 | ผ่าน USB ST-Link (Virtual COM Port) |

---
**ข้อควรระวัง:** 
1. ขา **PB12 (CN7-7)** และ **PC3 (CN9-5)** ต้องเว้นไว้เพื่อระบบเสียง ห้ามนำไปใช้สแกนคีย์บอร์ด
2. การต่อสาย Matrix แนะนำให้ใช้สายแพที่สั้นที่สุดเพื่อลด Noise ในการอ่านค่า Velocity
