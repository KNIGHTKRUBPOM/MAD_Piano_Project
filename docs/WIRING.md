# 🧶 Project Piano - Wiring Guide (V6.1 Matrix Edition)

คู่มือการเดินสายไฟระหว่าง **AKAI MPK mini MK3** และ **STM32F767 (Nucleo-144)** โดยเน้นการใช้ขาแถวใน (Inside Pins) เพื่อเชื่อมต่อกับ Screw Terminal

---

## 🎹 1. Keyboard Matrix (8x8)
เชื่อมต่อสายแพจาก Keyboard Bed เข้ากับ Screw Terminal ที่ต่อกับขา **Inside** ของ Morpho Header (ขาเลขคี่)

### 🚥 Rows (Outputs)
| AKAI Row | STM32 Pin | Header Pin (Inside) | Port | Note |
| :--- | :--- | :--- | :--- | :--- |
| **ROW 0** | **PC6** | **CN7-1** | GPIOC | S1 (Contact 1) |
| **ROW 1** | **PB15** | **CN7-3** | GPIOB | S2 (Contact 2) |
| **ROW 2** | **PB13** | **CN7-5** | GPIOB | S1 |
| **ROW 3** | **PA15** | **CN7-9** | GPIOA | S2 |
| **ROW 4** | **PC7** | **CN7-11** | GPIOC | S1 |
| **ROW 5** | **PB5** | **CN7-13** | GPIOB | S2 |
| **ROW 6** | **PB3** | **CN7-15** | GPIOB | S1 |
| **ROW 7** | **PA4** | **CN7-17** | GPIOA | S2 |

### 🚦 Columns (Inputs)
| AKAI Col | STM32 Pin | Header Pin (Inside) | Port |
| :--- | :--- | :--- | :--- |
| **COL 0** | **PB4** | **CN7-19** | GPIOB |
| **COL 1** | **PC2** | **CN10-9** | GPIOC |
| **COL 2** | **PF4** | **CN10-11** | GPIOF |
| **COL 3** | **PB6** | **CN10-13** | GPIOB |
| **COL 4** | **PB2** | **CN10-15** | GPIOB |
| **COL 5** | **PD13** | **CN10-19** | GPIOD |
| **COL 6** | **PD12** | **CN10-21** | GPIOD |
| **COL 7** | **PD11** | **CN10-23** | GPIOD |

---

## 🔊 2. Audio DAC (PCM5102A)
| DAC Pin | STM32 Pin | Header Pin | Note |
| :--- | :--- | :--- | :--- |
| **BCK** | **PB10** | CN10-32 | SPI2_SCK |
| **DIN** | **PC3** | CN9-5 | SPI2_MOSI |
| **LCK (WS)** | **PB12** | CN7-7 | SPI2_NSS |
| **SCK** | **GND** | - | ต่อลง Ground เพื่อใช้ Internal Clock |
| **VCC** | **5V** | CN10-6 | จ่ายไฟ 5V |
| **GND** | **GND** | CN10-22 | กราวด์ร่วม |

---

## 📺 3. TFT Display (SPI1)
| Screen Pin | STM32 Pin | Header Pin |
| :--- | :--- | :--- |
| **CS** | **PD14** | CN7-16 |
| **RESET** | **PF12** | CN7-20 |
| **DC** | **PD15** | CN7-18 |
| **MOSI** | **PD7** | CN9-2 |
| **SCK** | **PA5** | CN7-10 |
| **LED** | **PB1** | CN10-24 |

---

## 🎛️ 4. Controls
| Component | STM32 Pin | Header Pin |
| :--- | :--- | :--- |
| **Master VR** | **PA3** | CN9-1 |
| **Status LED** | **PB14** | LD3 (Red) |
| **Test Button** | **PC13** | User Button (Blue) |
