# คู่มือการพัฒนาและดูแลรักษาระบบ (Developer Guide)

เอกสารนี้สำหรับนักพัฒนาและผู้ดูแลโครงการ **Open Law Data Thailand (`open-law-data-web`)** อธิบายขั้นตอนการรันระบบในเครื่อง มาตรฐานการออกแบบ UI ขั้นตอนการเพิ่มข้อมูล/แหล่งข้อมูลใหม่ รวมถึงรายการจุดที่ควรปรับปรุง (Technical Audit & Backlog) สำหรับการพัฒนาต่อ

---

## 1. การตั้งค่าและรันเว็บไซต์ในเครื่อง (Local Development Setup)

โปรเจกต์นี้เป็น **Zero-Build Static Website** ที่เก็บไฟล์ทั้งหมดไว้ในโฟลเดอร์ [`public/`](../public/) ไม่จำเป็นต้องใช้ `npm install` หรือขั้นตอน Build ใดๆ

### ทำไมต้องรันผ่าน HTTP Server?
ภายใน [`public/index.html`](../public/index.html#L1142) มีการใช้คำสั่ง `fetch('./data/showcases.toml')` เพื่อดึงข้อมูล Community Showcase หากดับเบิลคลิกเปิดไฟล์ `public/index.html` ผ่านโปรโตคอล `file://` เบราว์เซอร์จะบล็อกการดึงไฟล์ด้วยนโยบาย CORS

### คำสั่งรัน Local Server
```bash
# ทางเลือกที่ 1: ใช้ Python 3 (มีมาให้ใน macOS / Linux)
python3 -m http.server 8000 --directory public

# ทางเลือกที่ 2: ใช้ Node.js
npx serve public -l 8000
```
* เปิดเข้าใช้งานหน้าเว็บหลักที่: `http://localhost:8000`
* เปิดทดสอบหน้า Discord Redirect ที่: `http://localhost:8000/discord/`

---

## 2. มาตรฐานการออกแบบและระบบ UI (Design System & Tokens)

### 2.1 ฟอนต์และสีหลัก (Typography & Color Palette)
กำหนดไว้ใน Tailwind Configuration (`public/index.html#L49-L64`):
* **Font Family**: `Prompt` (Google Fonts, น้ำหนัก `300`, `400`, `500`, `600`, `700`)
* **Custom Colors**:
  * `thai-blue`: `#1e3a8a` — สีน้ำเงินเข้มหลักของโครงการ (ปุ่มหลัก, ไอคอน, ตัวเลขสถิติ)
  * `thai-dark`: `#0f172a` — สีพื้นหลังส่วน Footer
  * `tech-accent`: `#3b82f6` — สีฟ้าเน้นคำว่า `Data` ในโลโก้ `OpenLawData`
* **Gradient Text**: `.gradient-text` (`linear-gradient(135deg, #2563eb 0%, #9333ea 100%)`) สำหรับข้อความเน้นใน Hero Section

### 2.2 ไลบรารีที่โหลดผ่าน CDN (`<head>`)
1. **Tailwind CSS v3 CDN**: `https://cdn.tailwindcss.com`
2. **Phosphor Icons Web**: `https://unpkg.com/@phosphor-icons/web` (ใช้งานผ่านคลาส `<i class="ph-fill ph-scales"></i>` หรือ `ph-bold`)
3. **fast-toml (`v0.5.4`)**: `https://unpkg.com/fast-toml@0.5.4/fast-toml.js` (กำหนด `var module = { exports: {} }` ก่อนโหลด และผูกเข้ากับตัวแปร `window.toml` ในบรรทัดที่ `45–47`)

---

## 3. คู่มือการขยายฟีเจอร์หลัก (Common Development Tasks)

### 3.1 วิธีเพิ่มแหล่งข้อมูลใหม่ในระบบตรวจสอบสถานะ (`#update-popup`)
เมื่อมีชุดข้อมูลใหม่บน Hugging Face (เช่น ข้อมูลประมวลกฎหมายจากสำนักงานคณะกรรมการกฤษฎีกา `ocs-*`):

1. เปิดไฟล์ [`public/index.html`](../public/index.html#L951-L959) ไปที่ตัวแปร `sources`:
   ```javascript
   const sources = [
       {
           name: 'ราชกิจจานุเบกษา',
           key: 'soc-ratchakitcha',
           url: 'https://huggingface.co/datasets/open-law-data-thailand/soc-ratchakitcha/resolve/main/latest_update.json',
           icon: 'ph-scroll',
           color: 'blue'
       },
       // เพิ่มแหล่งข้อมูลใหม่ที่นี่:
       {
           name: 'ประมวลกฎหมาย (กฤษฎีกา)',
           key: 'ocs-law-codes', // ชื่อ repo บน Hugging Face
           url: 'https://huggingface.co/datasets/open-law-data-thailand/ocs-law-codes/resolve/main/latest_update.json',
           icon: 'ph-scales',
           color: 'indigo'
       }
   ];
   ```
2. **ตรวจสอบลิงก์ไฟล์เอกสาร (`L1045-L1051`)**:
   ปัจจุบันโค้ดสร้าง `pdfUrl` เฉพาะเมื่อ `source.key === 'soc-ratchakitcha' && record.pdf_file` หากแหล่งข้อมูลใหม่มีโครงสร้างโฟลเดอร์ไฟล์ต้นฉบับต่างออกไป ให้เพิ่มเงื่อนไขสร้าง URL สำหรับ `source.key` ใหม่ในส่วนนั้น
3. **ข้อควรระวังเรื่องสีของ Tailwind CDN**:
   ในโค้ดสร้างการ์ด (`L1056-L1084`) มีการใช้ Template Literal เช่น `bg-${source.color}-50`, `text-${source.color}-600`, `border-${source.color}-100`, `bg-${source.color}-600`, `hover:bg-${source.color}-700` เนื่องจาก Tailwind CDN สแกนชื่อคลาสแบบเต็มสตริงจาก DOM หากใช้สีใหม่ที่ยังไม่เคยมีคลาสเต็มเหล่านี้อยู่ในหน้า HTML มาก่อน แนะนำให้เปลี่ยนเป็น Lookup Object ของคลาสเต็มแทน หรือใส่คลาสสีไว้ใน Safelist

### 3.2 วิธีเพิ่มหรือแก้ไขการ์ดหน่วยงานพันธมิตร (`#partners` และ `#credits`)
* **พันธมิตรภาครัฐและผู้ริเริ่มโครงการ**: อยู่ใน `<section id="partners">` (`public/index.html#L557-L666`) ใช้ Grid 4 คอลัมน์ (`grid-cols-1 md:grid-cols-2 lg:grid-cols-4`)
* **พันธมิตรเทคโนโลยีเบื้องหลัง**: อยู่ใน `<section id="credits">` (`public/index.html#L669-L730`) ใช้ Grid 2 คอลัมน์ (`grid md:grid-cols-2`)
* ไฟล์โลโก้ทั้งหมดเก็บไว้ที่ `public/images/logo/` แนะนำให้ใช้ไฟล์ `.svg` หรือ `.png`/`.jpg` ขนาดสี่เหลี่ยมจัตุรัสหรือแนวนอนที่มีพื้นหลังโปร่งใส

---

## 4. รายการตรวจสอบทางเทคนิคและการปรับปรุงที่ดำเนินการแล้ว (Resolved Technical Audit)

| ลำดับ | หมวดหมู่ | สถานะ | รายละเอียดการปรับปรุง |
| :---: | :--- | :---: | :--- |
| **1** | **HTML Syntax (`#credits`)** | ✅ แก้ไขแล้ว | เพิ่มแท็กเปิด `<p class="text-gray-500 max-w-2xl mx-auto text-lg">` ในส่วน `#credits` ([`public/index.html#L646-L648`](../public/index.html#L646-L648)) |
| **2** | **Navigation Links** | ✅ แก้ไขแล้ว | เพิ่มลิงก์ `นวัตกรรมชุมชน (#showcase)` และ `โครงสร้างข้อมูล (#data-preview)` ทั้งใน Desktop และ Mobile พร้อมสั่งพับเมนูมือถืออัตโนมัติเมื่อกดลิงก์ ([`public/index.html#L165-L199`](../public/index.html#L165-L199)) |
| **3** | **Dead Code (`#partners`)** | ✅ แก้ไขแล้ว | ลบบล็อก `<!-- Private Sector Row -->` ที่ซ่อนไว้ (`hidden`) ออกจาก `#partners` |
| **4** | **License & Copyright** | ✅ แก้ไขแล้ว | ปรับข้อความ License ใน Footer เป็น `CC BY 4.0 (Data) · MIT (Code)` และอัปเดตปีลิขสิทธิ์เป็น `2568–2569` ([`public/index.html#L878-L884`](../public/index.html#L878-L884)) |
| **5** | **XSS Hardening & Accessibility** | ✅ แก้ไขแล้ว | เพิ่ม `escapeHtml()` และ `safeUrl()` ก่อนเรนเดอร์ข้อมูลลง `innerHTML`, เปลี่ยนคลาสสีไดนามิกเป็น `colorStyles` Map, และรองรับปุ่ม `Escape` เพื่อปิด `#update-popup` ([`public/index.html#L904-L1340`](../public/index.html#L904-L1340)) |
| **6** | **Absolute Open Graph Image URL** | ✅ แก้ไขแล้ว | เปลี่ยน `og:image` ทั้งใน `public/index.html` และ `public/discord/index.html` ให้ใช้ URL เต็ม `https://www.openlawdatathailand.org/images/og/og_image.png` |
| **7** | **Interactive Data Preview & OCS Status** | ✅ แก้ไขแล้ว | เพิ่มปุ่มสลับ Tab (`meta/` / `ocr/` / `hf cli`) พร้อมปุ่มคัดลอกในส่วน `#data-preview` และเพิ่มการ์ดสถานะ `coming_soon` สำหรับข้อมูลประมวลกฎหมาย (สำนักงานคณะกรรมการกฤษฎีกา) ใน `#update-popup` |
