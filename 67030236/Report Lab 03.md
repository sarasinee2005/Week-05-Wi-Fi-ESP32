## 6. ตารางบันทึกผลการทดลอง (Experiment Results)

ให้นักศึกษาบันทึกผลลัพธ์จากการสังเกตใน Serial Console ลงในตารางต่อไปนี้:

### 6.1 ตารางสรุปเปรียบเทียบผลการทดลองในระดับ Link Layer

| ข้อการทดลอง | สถานการณ์ทดสอบ | Event ที่ได้รับ | ผลการผูกสัมพันธ์ Link Layer | ค่า Association ID (AID) ที่ได้ | Reason Code (ถ้ามี) |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **5.3.1** | ร้องขอ Auth & Assoc กับ AP มีอยู่จริง | WIFI_EVENT_STA_CONNECTED | สำเร็จ (COMPLETED / PASSED) | 2 (หรือ 3 ในบันทึก Log) | N/A |
| **5.3.2** | ร้องขอ Auth & Assoc กับ AP ไม่มีอยู่จริง | WIFI_EVENT_STA_DISCONNECTED | ไม่สำเร็จ (Failed) | N/A | 201 (WIFI_REASON_NO_AP_FOUND)  |

### 6.2 บันทึกข้อมูล Link Layer จาก Event `WIFI_EVENT_STA_CONNECTED` (ข้อ 5.3.1)

| พารามิเตอร์ Link Layer | ค่าที่อ่านได้จริงจาก Forensic Log |
| :--- | :--- |
| **SSID** | POCO X7 Pro |
| **BSSID (MAC Address)** | 16:FB:5D:E5:3D:C7 |
| **Channel** | 13 |
| **Auth Mode Enum** | 3 |
| **Association ID (AID)** | 3 |

---
```c
### 6.3 ผลการรัน
rst:0x1 (POWERON_RE�H(
B��J���� Enabling RNG early entropy source...
I (49) boot: Partition Table:
I (51) boot: ## Label            Usage          Type ST Offset   Length
I (58�ets Jul 29 2019 12:21:46

rst:0x1 (POWERON_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:2
load:0x3fff0030,len:6380
ho 0 tail 12 room 4
load:0x40078000,len:15916
load:0x40080400,len:3860
--- 0x40080400: _invalid_pc_placeholder at C:/Users/hp/esp/v5.5.1/esp-idf/components/xtensa/xtensa_vectors.S:2235
entry 0x40080638
--- 0x40080638: call_start_cpu0 at C:/Users/hp/esp/v5.5.1/esp-idf/components/bootloader/subproject/main/bootloader_start.c:25
I (29) boot: ESP-IDF v5.5.1 2nd stage bootloader
I (29) boot: compile time Aug  3 2026 10:06:14
I (29) boot: Multicore bootloader
I (31) boot: chip revision: v3.1
I (33) boot.esp32: SPI Speed      : 40MHz
I (37) boot.esp32: SPI Mode       : DIO
I (41) boot.esp32: SPI Flash Size : 2MB
I (44) boot: Enabling RNG early entropy source...
I (49) boot: Partition Table:
I (51) boot: ## Label            Usage          Type ST Offset   Length
I (58) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (64) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (71) boot:  2 factory          factory app      00 00 00010000 00100000
I (77) boot: End of partition table
I (81) esp_image: segment 0: paddr=00010020 vaddr=3f400020 size=19e28h (106024) map
I (124) esp_image: segment 1: paddr=00029e50 vaddr=3ffb0000 size=03eech ( 16108) load
I (130) esp_image: segment 2: paddr=0002dd44 vaddr=40080000 size=022d4h (  8916) load
I (134) esp_image: segment 3: paddr=00030020 vaddr=400d0020 size=860f0h (549104) map
I (323) esp_image: segment 4: paddr=000b6118 vaddr=400822d4 size=15b38h ( 88888) load
I (358) esp_image: segment 5: paddr=000cbc58 vaddr=50000000 size=00020h (    32) load
I (370) boot: Loaded app from partition at offset 0x10000
I (370) boot: Disabling RNG early entropy source...
I (380) cpu_start: Multicore app
I (389) cpu_start: Pro cpu start user code
I (389) cpu_start: cpu freq: 160000000 Hz
I (389) app_init: Application information:

```

## 7. คำถามท้ายการทดลอง (Post-Lab Questions)

1. **Association ID (AID)** คืออะไร มีบทบาทอย่างไรใน Phase 3 และส่งคืนมาในโครงสร้างข้อมูลตัวแปรใด?
   answer : ความหมายและบทบาท: AID คือ หมายเลขประจำตัวชั่วคราวที่ Access Point (AP) ออกให้กับอุปกรณ์ลูกข่าย (STA) แต่ละตัวในขณะทำ Association
   เพื่อใช้ระบุตัวตนของ Station นั้นๆ ในเครือข่าย BSS (เช่น ใช้จัดการคิวรับส่งข้อมูล และควบคุมโหมดประหยัดพลังงาน Power Save / TIM Buffer)
โครงสร้างข้อมูลตัวแปร: ส่งคืนมาผ่าน Struct wifi_event_sta_connected_t ในฟิลด์ aid (เข้าถึงได้จาก event_data ของ Event WIFI_EVENT_STA_CONNECTED)
2. เหตุใดการเชื่อมต่อ Wi-Fi ความปลอดภัยแบบ WPA2-PSK จึงสามารถผ่าน Phase 2 (Authentication) และ Phase 3 (Association) จนเกิด Event `WIFI_EVENT_STA_CONNECTED` ได้สำเร็จ แม้ผู้ใช้จะป้อนรหัสผ่าน (Password) ผิด?
   answer : เพราะในระดับ 802.11 Link Layer Phase 2 (Authentication) ของ WPA2 จะเป็นแบบ Open System Authentication (เป็นการยืนยันตัวตนระดับฮาร์ดแวร์เบื้องต้น ไม่ได้ใช้อัลกอริทึมตรวจสอบ Password) ส่วนการตรวจสอบรหัสผ่าน (Pre-Shared Key / PSK) จริงๆ จะเกิดขึ้นใน Phase 4 (4-Way Handshake / Port Authorization) ดังนั้นใน Phase 2 และ Phase 3 อุปกรณ์จึงผ่านกระบวนการผูกสัมพันธ์สำเร็จและเกิด Event WIFI_EVENT_STA_CONNECTED ได้ตามปกติ แต่จะไปถูกตัดการเชื่อมต่อ (Disconnect) ใน Phase 4 ถ้ารหัสผ่านไม่ถูกต้อง
3. หาก Router มีการตั้งค่า **MAC Address Filtering** (อนุญาตเฉพาะ MAC ที่ลงทะเบียน) ESP32 จะล้มเหลวในเฟสใด และจะส่ง Disconnect Reason Code ใดออกมา?
   answer : เฟสที่ล้มเหลว: จะล้มเหลวตั้งแต่ Phase 3 (Association Phase) หรือ Phase 2 (Authentication Phase) ขึ้นอยู่กับการ 구현 ของ AP
Disconnect Reason Code: มักจะได้รับ Reason Code 2 (WIFI_REASON_AUTH_EXPIRE) หรือ 6 / 7 (WIFI_REASON_NOT_AUTHED / WIFI_REASON_NOT_ASSOCED) หรือ Reason Code เฉพาะทาง เช่น 24 (WIFI_REASON_CONNECTION_FAIL)
4. สรุปความแตกต่างสำคัญระหว่างจุดสิ้นสุดของ **Phase 3 (Link-Layer Connected)** กับจุดสิ้นสุดของ **Phase 5 (IP Address Assigned)**
   answer : จุดสิ้นสุด Phase 3 (Link-Layer Connected): อุปกรณ์ ESP32 และ AP สามารถสื่อสารกันในระดับฮาร์ดแวร์/เฟรม 802.11 ได้แล้ว (สถาปัตยกรรม Layer 2 - Data Link Layer) แต่ยังไม่สามารถรับ-ส่งข้อมูลบนเครือข่ายอินเทอร์เน็ตได้ และยังไม่มี IP Address
จุดสิ้นสุด Phase 5 (IP Address Assigned): อุปกรณ์ผ่านกระบวนการยืนยันรหัสผ่าน (4-Way Handshake) และได้รับ IP Address จาก DHCP Server เรียบร้อยแล้ว (สถาปัตยกรรม Layer 3 - Network Layer) ทำให้ ESP32 พร้อมสำหรับการสื่อสารผ่านโปรโตคอล TCP/IP, HTTP, MQTT หรือออกสู่อินเทอร์เน็ตได้สมบูรณ์
