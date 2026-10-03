# Project Piano: AKAI MPK mini MK3 x STM32F767 (Standalone)

## 🎹 Project Overview
การพัฒนาระบบควบคุมและ Audio Engine สำหรับ AKAI MPK mini MK3 โดยใช้ **STM32F767ZIT6** เพียงตัวเดียวเป็นทั้ง Input Controller, GUI Orchestrator และ Audio Engine (Standalone Mode)

---

## 🏗 Modular Architecture (Updated V6.1 - Matrix Edition)

### 1. STM32F767 (The All-in-One Engine)
- **High-Performance Audio:** 16-voice Polyphonic, 48kHz/16-bit I2S DMA Output (PCM5102A).
- **Audio Mixing:** ใช้ระบบ Cubic Soft Clipping และ Dynamic Headroom Management เพื่อรองรับการเล่น **Chord** โดยเสียงไม่แตก
- **Hardware Graphics Acceleration:** 
    - ใช้งาน **Hardware SPI1** วิ่งที่ความเร็ว 6.75MHz (Prescaler 32) เพื่อความเสถียรของ Touch.
- **8x8 Matrix Scanning:** สแกนปุ่มกดเปียโนโดยตรงผ่านขา Morpho **Inside Pins** (แถวใน) พร้อมระบบคำนวณ **Velocity** (S1/S2 contacts).

### 2. Feature Stack
- **Sound Engines:** 3 โหมดเสียง (GRAND PIANO, E-PIANO, STRINGS) เปลี่ยนได้ผ่าน Touchscreen.
- **Sequencer & Library:** บันทึกโน้ตเพลงได้ 4 Slots (สูงสุด 2,000 events ต่อเพลง) พร้อมระบบ **Persistent Storage** เซฟลง Flash Memory (Sector 11) อัตโนมัติ.
- **MP3-style Player:** หน้าจอเครื่องเล่นเพลงพร้อม Progress Bar, Play/Pause และ Stop.
- **Hardware Tester:** ปุ่มสีน้ำเงิน (PC13) สำหรับทดสอบเสียง (Note C4) และ LED สีแดง (LD3) แสดงสถานะ I2S DMA.

---

## ⚠️ Strict Mandates (กฎเหล็กในการพัฒนา)

1. **Pin Configuration:**
    - **ห้ามแก้ไข** รายการใน `ROW_PORTS/PINS` และ `COL_PORTS/PINS` ใน `keyboard_matrix.c` โดยพลการ เนื่องจากผูกกับ Screw Terminal บน Hardware
    - การกำหนดขา (Hardware Mapping) ต้องตรงกับ `docs/PINOUT.md` เสมอ
2. **STM32F7 Audio Standards:**
    - ต้องใช้ **Signed int16_t** สำหรับ Audio Buffer เพื่อให้ตรงกับมาตรฐาน I2S ของ PCM5102A
    - ต้องเปิดใช้งาน **PLLI2S** ใน SystemClock_Config เสมอ
3. **UI & Touch:**
    - ข้อความที่แสดงบนจอต้องเป็น **ตัวพิมพ์ใหญ่ (UPPERCASE)**
    - SPI1 Speed ห้ามเกิน 13MHz หากต้องการใช้งาน Touchscreen ให้เสถียร

---

## 📅 Roadmap & Progress
- [x] Phase 9: **Standalone STM32 Migration**
- [x] Phase 10: **Advanced Features**
    - [x] **8x8 Matrix Integration:** เปลี่ยนจาก Direct PE เป็น Matrix 8x8 บน Morpho Inside Pins.
    - [x] **Velocity & Polyphony:** รองรับน้ำหนักการกดและเล่นคอร์ด (Chord) ได้นุ่มนวล.
    - [x] **Flash Storage:** ระบบบันทึกเพลงลง Internal Flash (Persistence).
    - [x] **Multi-Mode Engine:** ระบบสลับเสียง Piano/E-Piano/Strings.
- [ ] Phase 11: **External I/O & Effects**
    - [ ] [dcr] **Advanced Audio Effects:** เพิ่ม Reverb และ Echo แบบสมบูรณ์.
    - [ ] [714] **SD Card Support:** ย้ายการเก็บ Library จาก Flash ไป SD Card เพื่อความจุที่มากขึ้น.

---

## 🆘 Troubleshooting (Technical Lessons)
1. **Siren Sound/Distortion:** เกิดจากส่งข้อมูล Unsigned uint16_t เข้า I2S ที่รอรับ Signed int16_t (แก้โดยใช้ Signed buffer)
2. **Calibration Loop on Boot:** เกิดจากขา PC13 (Active High) มี Noise ตอนเริ่มจ่ายไฟ (แก้โดยเพิ่ม Startup Delay 500ms)
3. **Volume Bar Not Moving:** เกิดจาก ADC Noise ไปทับซ้อนค่าจาก Touch (แก้โดยเพิ่ม Threshold Check > 50)
---
