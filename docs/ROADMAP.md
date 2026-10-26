# แผนการพัฒนาโครงการ (Project Roadmap & Next Steps)

เอกสารนี้สรุปพัฒนาการที่ผ่านมาของโครงการ **Open Law Data Thailand** และทิศทางการพัฒนาต่อยอดทั้งในส่วนของเว็บไซต์หน้าบ้าน (`open-law-data-web`) และระบบนิเวศข้อมูลกฎหมายเปิด

---

## ✅ เฟสที่ 1: รากฐานและชุดข้อมูลราชกิจจานุเบกษา (ดำเนินการเสร็จสิ้น)

* **ความร่วมมือกับสำนักเลขาธิการคณะรัฐมนตรี (สลค.)**: รวบรวมข้อมูลราชกิจจานุเบกษาย้อนหลังตั้งแต่ พ.ศ. 2428 ถึงปัจจุบัน กว่า 1.34 ล้านฉบับ (170GB+)
* **สถาปัตยกรรม Hot/Cold Storage บน Hugging Face**: แยกเก็บข้อมูลเป็น `pdf/` (3 เดือนล่าสุด), `zip/` (ย้อนหลังรายเดือน), `meta/` (Metadata) และ `ocr/` (AI OCR ภาษาไทยโดย iApp Technology)
* **เปิดตัวเว็บไซต์หลัก (`openlawdatathailand.org`)**: พัฒนา Stateless Web Portal บน GitHub Pages ภายใต้โครงสร้างพื้นฐานของ DGA (`DGA-Thailand/open-law-data`)
* **ระบบติดตามสถานะอัปเดตแบบ Real-time**: เชื่อมต่อหน้าเว็บเข้ากับ `latest_update.json` บน Hugging Face
* **เปิดตัว Community Showcase และชุมชน Discord**: เปิดพื้นที่ให้นักพัฒนาส่งผลงานต่อยอด (เช่น BigQuery Sync, Google Colab) ผ่าน `public/data/showcases.toml`

---

## 🚧 เฟสที่ 2: การขยายแหล่งข้อมูลและการยกระดับหน้าเว็บไซต์ (กำลังดำเนินการ / เตรียมพัฒนาต่อ)

### 2.1 การเชื่อมต่อชุดข้อมูลใหม่ (New Data Sources Onboarding)
* **สำนักงานคณะกรรมการกฤษฎีกา (OCS)**:
  * เพิ่มข้อมูลชุดประมวลกฎหมายจากสำนักงานคณะกรรมการกฤษฎีกาเข้าสู่คลังข้อมูลบน Hugging Face
  * อัปเดตตัวแปร `sources[]` ใน [`public/index.html`](../public/index.html#L952-L959) เพื่อให้ป้ายสถานะอัปเดตล่าสุดและหน้าต่าง `#update-popup` แสดงสถานะของข้อมูลกฤษฎีกาควบคู่กับราชกิจจานุเบกษา
  * เพิ่มตัวอย่างโครงสร้างข้อมูล (Data Preview / Data Dictionary) ของชุดข้อมูลกฤษฎีกาในส่วน `#data-preview`
* **คณะทำงานชุดที่ 2**: ขยายความร่วมมือไปยังหน่วยงานภาครัฐอื่นๆ ที่ถือครองข้อมูลสาธารณะด้านกฎหมายและระเบียบข้อบังคับ

### 2.2 การปรับปรุงเว็บไซต์ `open-law-data-web` (Web Portal Backlog)
* **UI/UX & Navigation**:
  * เพิ่มลิงก์เมนู `#showcase` (นวัตกรรมจากชุมชน) และ `#data-preview` (โครงสร้างข้อมูล) ลงในแถบนำทางด้านบนทั้ง Desktop และ Mobile
  * เพิ่มระบบค้นหาหรือกรองตามแท็ก (`#tags`) ในส่วน Community Showcase เมื่อมีจำนวนผลงานเพิ่มขึ้น
  * เพิ่มตัวอย่างโค้ด (Code Snippets / Tabs) บนหน้าเว็บสำหรับดึงข้อมูลด้วย `huggingface-cli`, `Python (datasets / pandas)` และ `BigQuery SQL`
* **Multi-Source SLA Dashboard**:
  * พัฒนาหน้าหรือส่วนแสดงผลสถานะความพร้อมใช้งาน (SLA & Freshness) ของแต่ละแหล่งข้อมูลอย่างชัดเจน
* **Code Quality & Security Hardening**:
  * แก้ไขจุดบกพร่องเล็กน้อยใน HTML/Meta Tags ตามที่ระบุไว้ใน [`docs/DEVELOPMENT.md`](DEVELOPMENT.md#4-รายการตรวจสอบทางเทคนิคและจุดที่ควรปรับปรุง-technical-audit--known-issues)
  * เพิ่มการทำ HTML Escaping และตรวจสอบ URL ในระบบแสดงผล `showcases.toml`
  * เพิ่ม GitHub Actions Workflow สำหรับตรวจสอบความถูกต้องของไฟล์ `public/data/showcases.toml` อัตโนมัติเมื่อมีผู้ส่ง Pull Request

---

## 🎯 เฟสที่ 3: การสร้างระบบนิเวศและมาตรฐานข้อมูลแห่งชาติ (ระยะกลาง–ยาว)

* **Open Law Data Hackathon**:
  * จัดกิจกรรม Hackathon ร่วมกับนักพัฒนา นิติกร Data Scientist และนักศึกษา เพื่อสร้างนวัตกรรมต้นแบบ เช่น AI Legal Advisor, ระบบตรวจสอบสัญญาอัตโนมัติ (Compliance Automation), และเครื่องมือวิเคราะห์ Regulatory Guillotine
* **การผลักดันมาตรฐานข้อมูลกฎหมายเปิดระดับประเทศ (National Open Law Standard Protocol)**:
  * ผลักดันให้กฎหมายและประกาศราชการทุกฉบับอยู่ในรูปแบบ Machine-Readable ตั้งแต่วันแรกที่ประกาศใช้ พร้อมมี Open API กลางตามมาตรฐานรัฐบาลดิจิทัล
