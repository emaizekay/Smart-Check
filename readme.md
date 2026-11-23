# SmartCheck Pro — Mobile & Tablet Trade-In Diagnostics

ระบบตรวจเช็กสภาพมือถือและแท็บเล็ต ประเมินเกรดมาตรฐานสากล (**A+ ถึง D**) และคำนวณราคารับซื้อ/เทิร์นแบบเรียลไทม์ ออกแบบสำหรับหน้าร้านรับซื้อโดยเฉพาะ — **ไม่ต้องผูก Apple ID / Google Account ไม่ต้องโหลดแอป และไม่ต้องใส่ซิมในเครื่องทดสอบ**

🔗 **Live Demo (GitHub Pages):** [https://emaizekay.github.io/my-website-check/](https://emaizekay.github.io/my-website-check/)

---

## ความสามารถหลัก (Key Features)

* **ตรวจฮาร์ดแวร์ครบ 15 ด่านผ่านเบราว์เซอร์:** ทัชสกรีน 48 ช่องขอบชนขอบ, สีจอ 5 สีเต็มจอ (ไม่ต้องกรอกตัวเลข), กล้องหลังครบทุกระยะ (`0.5x / 1x / 2x–5x / Macro`), ลำโพง/ไมค์/ระบบสั่น (รองรับ iOS), เซนเซอร์แยกอิสระ (`Proximity` และ `Gyroscope`), และด่านตรวจจอแท้แบบเร็ว 3 ขั้นตอน (30 วินาที)
* **ดูดราคารับซื้อสดอัตโนมัติทุกวัน (GitHub Actions):** ดึงตารางราคาจริงจาก Studio7 และ BaNANA ประเทศไทยลงไฟล์ `live_prices.json` ทุกเช้าเวลา 07:00 น. พร้อมเพิ่มรุ่นใหม่และปรับลดราคารุ่นเก่าตามเวลาอัตโนมัติ
* **โหมดเสียบสายหน้าคอม (USB Station — แบบ 3uTools):** อ่านรุ่นเครื่อง, โซน (`TH/A`), ความจุ, สี, IMEI/Serial, สุขภาพแบต % & รอบชาร์จจริง (รวมถึง iPad และ Samsung `mSavedBatteryAsoc`) ผ่าน `pymobiledevice3` (iOS) และ `ADB` (Android)
* **บันทึกประวัติขึ้น Google Sheets อัตโนมัติ:** รองรับการเชื่อมต่อ `Code.gs` เพื่อเก็บประวัติการตรวจ 21 คอลัมน์ (`Inspection_History`) และจัดการแค็ตตาล็อกราคาเองได้
* **พร้อม Plug-in เข้ากับระบบหลัก (WMS / POS):** มี `window.SmartCheckAPI` + `postMessage` (`SMARTCHECK_RESULT`) และรองรับการส่งค่าเริ่มต้นผ่าน URL Query

---

## โครงสร้างไฟล์ใน Repository

```text
my-website-check/
├── index.html                                # เว็บแอป SmartCheck Pro ฉบับสมบูรณ์ (Single-File)
├── live_prices.json                          # ตารางราคารับซื้อสด (อัปเดตอัตโนมัติทุกวัน)
├── Code.gs                                   # Google Apps Script สำหรับเชื่อมต่อ Google Sheets
├── manifest.json                             # PWA Manifest
├── AI_AGENT_HANDOFF_SPEC.md                  # สเปก Plug-in & JSON Schema สำหรับ AI Agent
├── SmartCheck_Pro_User_Guide.md              # คู่มือการใช้งานหน้าร้านฉบับละเอียด
├── scripts/
│   └── fetch_live_tradein_prices.py          # สคริปต์ Python ดูดราคาสดจาก Studio7 & BaNANA
├── .github/
│   └── workflows/
│       └── update_live_prices.yml            # GitHub Actions รันอัปเดตราคาทุกวัน 07:00 น. (ICT)
└── usb_station/
    ├── smartcheck_usb_server.py              # เซิร์ฟเวอร์เสียบสาย USB หน้าคอม (iOS + Android)
    ├── Start_SmartCheck_USB_Windows.bat      # ตัวเปิดใช้งานคลิกเดียวสำหรับ Windows
    ├── Start_SmartCheck_USB_Mac.command      # ตัวเปิดใช้งานคลิกเดียวสำหรับ macOS
    └── README_USB_STATION.md                 # คู่มือติดตั้งไดรเวอร์ USB บน Windows / Mac
```

---

## วิธีติดตั้งและเปิดใช้งานบน GitHub Pages

1. อัปโหลดไฟล์ทั้งหมดจาก `remobie_check_web_deploy.zip` ขึ้น Repository (รวมโฟลเดอร์ `.github/workflows/` และ `scripts/`)
2. ไปที่ **Settings > Pages** เลือก Branch `main` / `/ (root)` แล้วกด Save
3. เปิดระบบดูดราคาสดรายวัน: ไปที่แท็บ **Actions > Update Live Trade-In Prices (Daily)** แล้วกด **Run workflow** 1 ครั้ง (ตั้งเวลาอัปเดตอัตโนมัติทุกวันเวลา 07:00 น. เวลาไทย)

---

## การใช้งานโหมดเสียบสายหน้าคอม (USB Station)

1. เตรียมเครื่องคอมพิวเตอร์ (ทำครั้งเดียว):
   * **Windows:** ติดตั้ง Python 3 (ติ๊ก *Add Python to PATH*), ติดตั้งแอป **Apple Devices** หรือ **iTunes** (สำหรับไดรเวอร์ iPhone) และ **Android Platform-Tools (ADB)**
   * **macOS:** มีไดรเวอร์ Apple ในตัว หากต้องการเสียบ Android ให้รัน `brew install android-platform-tools`
2. ดับเบิลคลิก `Start_SmartCheck_USB_Windows.bat` (Windows) หรือ `Start_SmartCheck_USB_Mac.command` (Mac)
3. หน้าเว็บจะเปิดที่ `http://127.0.0.1:8765` อัตโนมัติ เสียบสายมือถือแล้วกด **สแกนผ่านสาย USB** ได้ทันที

---

## การฝังเป็น Plug-in ในระบบหลัก (`<iframe>` + `postMessage`)

```html
<iframe
  src="https://emaizekay.github.io/my-website-check/?embed=1&brand=Apple&model=iPhone%2015%20Pro&storage=256GB"
  allow="camera; microphone; fullscreen; accelerometer; gyroscope"
  style="width:100%; height:100vh; border:0;"
></iframe>

<script>
  window.addEventListener('message', (event) => {
    if (event.data?.source === 'SMARTCHECK_PRO' && event.data?.type === 'SMARTCHECK_RESULT') {
      console.log('SmartCheck Payload:', event.data.payload);
    }
  });
</script>
```

ดูสเปก JSON Schema และกฎหน้างานฉบับเต็มได้ที่ [AI\_AGENT\_HANDOFF\_SPEC.md](https://www.google.com/search?q=./AI_AGENT_HANDOFF_SPEC.md) และคู่มือพนักงานหน้าร้านได้ที่ [SmartCheck\_Pro\_User\_Guide.md](https://www.google.com/search?q=./SmartCheck_Pro_User_Guide.md)
