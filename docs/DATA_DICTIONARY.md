# โครงสร้างข้อมูลและพจนานุกรมข้อมูล (Data Dictionary & Schemas)

เอกสารนี้รวบรวมโครงสร้างข้อมูลทั้งหมดที่เกี่ยวข้องกับโครงการ **Open Law Data Thailand** ทั้งในส่วนของชุดข้อมูลบน Hugging Face และไฟล์ข้อมูลที่ใช้ภายในเว็บไซต์ (`open-law-data-web`)

---

## 1. โครงสร้างไดเรกทอรีบน Hugging Face Datasets

ชุดข้อมูลหลักชุดแรกของโครงการคือ **[`open-law-data-thailand/soc-ratchakitcha`](https://huggingface.co/datasets/open-law-data-thailand/soc-ratchakitcha)** ซึ่งจัดเก็บข้อมูลราชกิจจานุเบกษาโดยแยกตามประเภทการใช้งานดังนี้:

```text
soc-ratchakitcha/
├── latest_update.json               # ไฟล์สถานะล่าสุดสำหรับแสดงผลบนหน้าเว็บไซต์แบบ Real-time
├── meta/                            # ข้อมูลบรรณานุกรม (Metadata) แยกตามปี ค.ศ.
│   └── YYYY/
│       └── ...                      # ไฟล์ JSON / JSONL เก็บรายละเอียดชื่อเรื่อง เล่ม ตอน วันที่ประกาศ
├── ocr/                             # ข้อความที่สกัดจาก PDF ด้วย AI OCR (iApp Technology)
│   └── <provider>/
│       └── YYYY/
│           └── ...                  # ไฟล์ JSONL เก็บข้อความรายหน้า/รายฉบับ สำหรับงาน NLP & AI
├── pdf/                             # [Hot Data] ไฟล์ PDF ต้นฉบับรายฉบับ (ย้อนหลัง 3 เดือนล่าสุด)
│   └── YYYY/
│       └── YYYY-MM/
│           └── YYYY-NNNNNN.pdf
└── zip/                             # [Cold Data] ไฟล์ PDF ต้นฉบับย้อนหลังตั้งแต่ พ.ศ. 2428 บีบอัดรายเดือน
    └── YYYY/
        └── YYYY-MM.zip
```

---

## 2. โครงสร้างข้อมูล Metadata ของเอกสารราชกิจจานุเบกษา (`meta/`)

ข้อมูลบรรณานุกรมของเอกสารแต่ละฉบับมีโครงสร้างมาตรฐานดังนี้ (อ้างอิงจากส่วนแสดงตัวอย่างบนหน้าเว็บ [`public/index.html#L759-L814`](../public/index.html#L759-L814)):

### ตัวอย่าง JSON
```json
{
  "no": "92",
  "doctitle": "สารบัญ วันอาทิตย์ เดือนยี่...",
  "bookNo": "1",
  "section": "55",
  "category": "General",
  "publishDate": "1885-01-31",
  "pageNo": "479",
  "pdfFile": "1885-000092.pdf"
}
```

### คำอธิบายฟิลด์ (Field Definitions)

| ฟิลด์ (Field) | ชนิดข้อมูล | คำอธิบาย | ตัวอย่างค่า |
| :--- | :--- | :--- | :--- |
| `no` | `String` | รหัสอ้างอิงเอกสารในระบบต้นทาง | `"92"` |
| `doctitle` | `String` | ชื่อเรื่องของประกาศหรือกฎหมายในราชกิจจานุเบกษา | `"พระราชบัญญัติ..."` |
| `bookNo` | `String` | เล่มที่ของราชกิจจานุเบกษา | `"1"` |
| `section` | `String` | ตอนที่ของราชกิจจานุเบกษา | `"55"` |
| `category` | `String` | หมวดหมู่ของประกาศราชกิจจานุเบกษา (เช่น ก, ข, ค, ง หรือ General) | `"General"` |
| `publishDate` | `String (Date)` | วันที่ประกาศในรูปแบบ ISO 8601 (`YYYY-MM-DD`, ปีคริสต์ศักราช) | `"1885-01-31"` |
| `pageNo` | `String` | เลขหน้าเริ่มต้นของเอกสารในเล่ม | `"479"` |
| `pdfFile` | `String` | ชื่อไฟล์ PDF ต้นฉบับที่ตรงกับรายการนี้ | `"1885-000092.pdf"` |

---

## 3. โครงสร้างไฟล์สถานะอัปเดตล่าสุด (`latest_update.json`)

ไฟล์ `latest_update.json` ที่รากของแต่ละ Dataset Repository บน Hugging Face ทำหน้าที่เป็นสัญญาข้อมูล (Data Contract) ระหว่าง **Data Pipeline บน NAS** กับ **หน้าเว็บไซต์ [`public/index.html`](../public/index.html#L951-L1113)**

* **URL ปัจจุบัน**: `https://huggingface.co/datasets/open-law-data-thailand/soc-ratchakitcha/resolve/main/latest_update.json`

### ตัวอย่าง JSON
```json
{
  "last_updated_pipeline": "2026-01-19T14:30:00Z",
  "latest_record": {
    "doctitle": "ประกาศสำนักนายกรัฐมนตรี เรื่อง ...",
    "publishDate": "2026-01-19",
    "pdf_file": "2026-000123.pdf"
  }
}
```

### คำอธิบายฟิลด์และการนำไปใช้บนหน้าเว็บ

| ฟิลด์ (Field) | ชนิดข้อมูล | การใช้งานใน `public/index.html` |
| :--- | :--- | :--- |
| `last_updated_pipeline` | `String (ISO 8601 Timestamp)` | ใช้คำนวณเวลาอัปเดตล่าสุดของระบบ (`latestPipelineDate`) แสดงบนป้าย `#latest-update-text` และตรวจสอบว่าเกิน 72 ชั่วโมงหรือไม่ (`L976-L1006`) |
| `latest_record` | `Object` | ข้อมูลของเอกสารฉบับล่าสุดที่ถูกดึงเข้าสู่ระบบ |
| `latest_record.doctitle` | `String` | ชื่อเรื่องเอกสารล่าสุด แสดงในการ์ดภายใน Modal `#update-popup` (`L1080`) |
| `latest_record.publishDate` | `String (YYYY-MM-DD)` | วันที่ประกาศของเอกสารล่าสุด ใช้แปลงเป็นวันที่ภาษาไทย (`th-TH`) และใช้แยกปี (`year`) กับเดือน (`month`) เพื่อสร้างลิงก์ PDF (`L1038-L1051`) |
| `latest_record.pdf_file` | `String` | ชื่อไฟล์ PDF ล่าสุด ใช้ประกอบเป็น URL: `.../resolve/main/pdf/${year}/${year}-${month}/${record.pdf_file}` (`L1047-L1050`) *(หมายเหตุ: ใน `latest_update.json` ใช้ชื่อคีย์ `pdf_file` แบบ snake_case ต่างจาก `pdfFile` ในตัวอย่าง Metadata)* |

---

## 4. โครงสร้างไฟล์ Community Showcase ([`public/data/showcases.toml`](../public/data/showcases.toml))

ไฟล์ `public/data/showcases.toml` เก็บรายการผลงานจากชุมชนนักพัฒนาในรูปแบบ **TOML Array of Tables (`[[projects]]`)** ซึ่งจะถูกอ่านและแปลงเป็น JSON บนฝั่งเบราว์เซอร์ด้วยไลบรารี `fast-toml@0.5.4` (`public/index.html#L1137-L1223`)

### ตัวอย่างรายการใน `showcases.toml`
```toml
[[projects]]
title = "Open Law Data on BigQuery"
author = "dtinth"
author_url = "https://github.com/dtinth"
description = "Syncs OCR data to Google BigQuery daily. Allows developers to query 1.3M+ records using SQL without downloading large files."
tags = ["BigQuery", "SQL"]
url = "https://github.com/dtinth/openlawdata-bigquery"
featured = false
date = "2026-01-18"
```

### คำอธิบายฟิลด์และการเรียงลำดับ

| ฟิลด์ (Field) | ชนิดข้อมูล | จำเป็น | พฤติกรรมการแสดงผลใน `renderCards()` |
| :--- | :--- | :---: | :--- |
| `title` | `String` | ✅ | แสดงเป็นหัวข้อของการ์ด พร้อมลิงก์ไปยัง `url` |
| `author` | `String` | ✅ | แสดงชื่อผู้พัฒนาที่ส่วนท้ายของการ์ดคู่กับไอคอนผู้ใช้ |
| `author_url` | `String` | ✅ | ลิงก์คลิกที่ชื่อ `author` (เปิดแท็บใหม่ `target="_blank"`) |
| `description` | `String` | ✅ | คำอธิบายโปรเจกต์ ตัดบรรทัดอัตโนมัติที่ 3 บรรทัดด้วยคลาส `line-clamp-3` |
| `tags` | `Array<String>` | ✅ | แสดงเป็นป้ายกำกับ `#tag` ด้านล่างคำอธิบาย |
| `url` | `String` | ✅ | ลิงก์หลักของผลงานทั้งที่หัวข้อโปรเจกต์และปุ่ม `View Project ->` |
| `featured` | `Boolean` | ➖ | หากเป็น `true` จะแสดงไอคอนดาวสีทอง (`ph-star`) และถูกจัดเรียงไว้ลำดับต้นสุด (หากไม่ระบุจะมีค่าเทียบเท่า `false`) |
| `date` | `String (YYYY-MM-DD)` | ✅ | ใช้สำหรับเรียงลำดับโปรเจกต์จากวันที่ใหม่สุดไปเก่าสุด (`new Date(b.date) - new Date(a.date)`) |

---

## 5. ตัวอย่างคำสั่งดึงข้อมูลผ่าน Hugging Face CLI (`hf download`)

การจัดโครงสร้างแบบแยกส่วนช่วยให้สามารถใช้ Glob Pattern (`*`, `?`) เลือกดาวน์โหลดเฉพาะส่วนที่ต้องการได้โดยไม่ต้องโหลดทั้ง 170GB+:

```bash
# 1. สำหรับงาน AI / NLP / LLM: ดึงเฉพาะ OCR Text และ Metadata ของยุค 2020s (ค.ศ. 2020-2029)
hf download open-law-data-thailand/soc-ratchakitcha --repo-type dataset --include "meta/202?/*" --local-dir "downloads"
hf download open-law-data-thailand/soc-ratchakitcha --repo-type dataset --include "ocr/*/202?/*" --local-dir "downloads"

# 2. สำหรับงานวิเคราะห์ข้อมูลและสถิติ (Data Analytics): ดึงเฉพาะ Metadata ทุกปี
hf download open-law-data-thailand/soc-ratchakitcha --repo-type dataset --include "meta/*/*" --local-dir "downloads"

# 3. สำหรับผู้ต้องการไฟล์ PDF ต้นฉบับล่าสุด (Hot Data 3 เดือนล่าสุด)
hf download open-law-data-thailand/soc-ratchakitcha --repo-type dataset --include "pdf/*/*/*" --local-dir "downloads"

# 4. สำหรับงานจดหมายเหตุ (Archivist): ดึงไฟล์ ZIP ต้นฉบับย้อนหลัง (สามารถระบุปีแทน * ได้ เช่น "zip/2024/*")
hf download open-law-data-thailand/soc-ratchakitcha --repo-type dataset --include "zip/*/*" --local-dir "downloads"
```
