# Interrupt and Peripheral Modules Documentation - Project Piano

เอกสารสรุปการใช้งาน Interrupt Service Routines (ISRs) และ Peripheral Modules สำหรับโปรเจกต์ AKAI MPK mini MK3 x STM32F767 (Standalone)

---

## ⚡ Interrupt Module 1: Audio Engine & I2S DMA
โมดูลหลักที่ใช้ในการขับเคลื่อนเสียง (Audio Engine) แบบ Real-time

*   **Interrupt Service Routine (ISR):**
    *   `DMA1_Stream4_IRQHandler`
    *   `SPI2_IRQHandler`
*   **Callback Functions:**
    *   `HAL_I2S_TxHalfCpltCallback(I2S_HandleTypeDef *hi2s)`
    *   `HAL_I2S_TxCpltCallback(I2S_HandleTypeDef *hi2s)`
*   **รายละเอียดการทำงาน:**
    ใช้ระบบ **Double Buffering** ผ่าน I2S DMA (Circular Mode) โดยเมื่อข้อมูลใน Buffer ครึ่งแรกถูกส่งออกไปครบจะเกิด `HalfCpltCallback` และเมื่อครบทั้งหมดจะเกิด `CpltCallback` เพื่อเรียกฟังก์ชัน `AudioEngine_Process` มาคำนวณสัญญาณเสียง (Sine/Square wave mixing) ในช่วงเวลาที่เหลือ ทำให้เสียงออกต่อเนื่องที่ความถี่ 48kHz/16-bit โดยไม่สะดุด

---

## ⚡ Interrupt Module 2: System Timebase & Sequencer
โมดูลฐานเวลาสำหรับระบบ UI, การควบคุมระดับเสียง และการบันทึกโน้ต

*   **Interrupt Service Routine (ISR):**
    *   `SysTick_Handler`
*   **Callback Functions:**
    *   `HAL_IncTick()`
*   **รายละเอียดการทำงาน:**
    เป็นหัวใจหลักของ Timebase (1ms) ในระบบ STM32 ใช้สำหรับการทำ Soft-interrupt ใน Loop หลักเพื่อ:
    1.  ประมวลผลระบบ **Sequencer** (บันทึกและเล่นโน้ตตาม Timestamp)
    2.  ใช้สำหรับ `HAL_GetTick()` ในการหน่วงเวลา (Polling) อ่านค่า ADC จาก Volume Potentiometer ทุกๆ 100ms
    3.  จัดการ debounce และหน่วงเวลาในการสแกนปุ่มกด Matrix 8x8

---

## 🛠️ โมดูลอื่นๆ ที่ใช้งาน (Other Peripheral Modules)

*   **I2S (SPI2):** รับหน้าที่เป็น Digital Audio Interface ส่งสัญญาณเสียงไปยัง PCM5102A DAC แบบ 16-bit Signed Integer
*   **SPI (SPI1):** ใช้งานร่วมกับจอ TFT LCD ความละเอียด 320x240 และใช้ระบบ Bit-banging (GPIO) ในการอ่านพิกัดสัมผัสจาก Touch Controller
*   **ADC (ADC1):** ใช้งาน Channel 3 (PA3) ในโหมด Polling เพื่ออ่านค่าจาก VR (Potentiometer) มาปรับระดับ Master Volume ของระบบ
*   **UART (USART3):** ทำหน้าที่ส่ง Debug Log (115200 bps) เพื่อตรวจสอบสถานะการทำงาน, ข้อมูล Note On/Off และการเข้าถึงหน่วยความจำ
*   **FLASH Memory:** ใช้การเข้าถึง Internal Flash (Sector 11) เพื่อบันทึกข้อมูลเพลง (MIDI Events) ให้คงอยู่แม้ปิดเครื่อง (Persistent Storage)
*   **GPIO (8x8 Matrix):** ใช้ขา Morpho (Inside Pins) ทำการสแกน Matrix แบบ Row/Column เพื่อรองรับการเล่น Polyphony และคำนวณน้ำหนักการกด (Velocity)
