# โครงการ Open Law Data Thailand

![Status](https://img.shields.io/badge/status-live-green)
[![Deploy Static Content to Pages](https://github.com/DGA-Thailand/open-law-data/actions/workflows/static.yml/badge.svg)](https://github.com/DGA-Thailand/open-law-data/actions/workflows/static.yml)
[![Hugging Face Datasets](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-open--law--data--thailand-yellow)](https://huggingface.co/open-law-data-thailand)
[![Discord](https://img.shields.io/badge/Discord-Join%20Community-5865F2?logo=discord&logoColor=white)](https://www.openlawdatathailand.org/discord)
![License ข้อมูล](https://img.shields.io/badge/Data%20License-CC%20BY%204.0-blue.svg)
![License โค้ด](https://img.shields.io/badge/Code%20License-MIT-lightgrey.svg)

**Open Law Data Thailand** คือโครงการความร่วมมือระหว่างภาครัฐ ภาคเอกชน และภาคประชาสังคม ที่มุ่งมั่นสร้างฐานข้อมูลกฎหมายกลางของประเทศไทยที่สมบูรณ์ เข้าถึงง่าย และอยู่ในรูปแบบที่คอมพิวเตอร์สามารถอ่านและประมวลผลต่อได้ (**Machine-Readable**) เพื่อปลดล็อกศักยภาพของข้อมูลกฎหมายไทยสู่นวัตกรรม Legal Tech และ AI

🌐 **เว็บไซต์หลัก**: [https://www.openlawdatathailand.org](https://www.openlawdatathailand.org)  
🤗 **คลังข้อมูล (Hugging Face)**: [https://huggingface.co/open-law-data-thailand](https://huggingface.co/open-law-data-thailand)  
💬 **ชุมชนนักพัฒนา (Discord)**: [https://www.openlawdatathailand.org/discord](https://www.openlawdatathailand.org/discord)

---

## 📊 ภาพรวมโครงการและชุดข้อมูล

| ตัวชี้วัด | รายละเอียด |
| :--- | :--- |
| **ข้อมูลย้อนหลัง** | ตั้งแต่ พ.ศ. 2428 (ค.ศ. 1885) – ปัจจุบัน |
| **จำนวนเอกสาร** | มากกว่า 1.34 ล้านฉบับ |
| **ขนาดฐานข้อมูลรวม** | มากกว่า 170 GB (PDF ต้นฉบับ, Metadata JSON และ OCR Text JSONL) |
| **รอบการอัปเดต** | อัตโนมัติทุก 24 ชั่วโมง |

### ชุดข้อมูลที่เปิดให้บริการ
1. **[`soc-ratchakitcha`](https://huggingface.co/datasets/open-law-data-thailand/soc-ratchakitcha)** — ข้อมูลราชกิจจานุเบกษา โดยความอนุเคราะห์จากสำนักเลขาธิการคณะรัฐมนตรี (สลค.) ประกอบด้วยไฟล์ต้นฉบับ (`pdf/`, `zip/`), ข้อมูลบรรณานุกรม (`meta/`), และข้อความที่ผ่านการทำ AI OCR ภาษาไทย (`ocr/`)
2. **ข้อมูลประมวลกฎหมาย (อยู่ระหว่างขยายผล)** — โดยความร่วมมือกับสำนักงานคณะกรรมการกฤษฎีกา (OCS)

---

## ⚡ การดึงข้อมูลไปใช้งาน (Quickstart)

ข้อมูลทั้งหมดถูกจัดโครงสร้างแบบแยกส่วนบน Hugging Face Datasets สามารถใช้ `huggingface-cli` (`hf`) ดึงเฉพาะส่วนที่ต้องการด้วย Glob Pattern ได้ทันที:

```bash
# 1. ดึงเฉพาะ Metadata ทั้งหมด (สำหรับงาน Data Analytics / Visualization)
hf download open-law-data-thailand/soc-ratchakitcha --repo-type dataset --include "meta/*/*" --local-dir "downloads"

# 2. ดึงเฉพาะข้อความ OCR ของทศวรรษ 2020s (สำหรับงาน AI / NLP / RAG)
hf download open-law-data-thailand/soc-ratchakitcha --repo-type dataset --include "ocr/*/202?/*" --local-dir "downloads"

# 3. ดึงไฟล์ PDF ต้นฉบับล่าสุด (Hot Data 3 เดือนล่าสุด)
hf download open-law-data-thailand/soc-ratchakitcha --repo-type dataset --include "pdf/*/*/*" --local-dir "downloads"
```

ดูรายละเอียดโครงสร้างไฟล์และฟิลด์ข้อมูลเพิ่มเติมได้ที่ **[`docs/DATA_DICTIONARY.md`](docs/DATA_DICTIONARY.md)**

---

## 📂 โครงสร้างคลังข้อมูล (Repository Structure)

คลังข้อมูลนี้ (`open-law-data-web`) ทำหน้าที่เป็น **Stateless Web Portal** ของโครงการ ซึ่งถูก Deploy อัตโนมัติผ่าน GitHub Pages:

```text
.
├── .github/
│   ├── workflows/
│   │   └── static.yml               # GitHub Actions สำหรับ Deploy โฟลเดอร์ public/ ขึ้น GitHub Pages
│   └── PULL_REQUEST_TEMPLATE.md     # แม่แบบสำหรับการส่ง Pull Request
├── docs/
│   ├── ARCHITECTURE.md              # สถาปัตยกรรมระบบ (Data Pipeline + Stateless Web Portal)
│   ├── DATA_DICTIONARY.md           # โครงสร้างข้อมูล (Dataset, latest_update.json, showcases.toml)
│   ├── DEVELOPMENT.md               # คู่มือการพัฒนาเว็บไซต์ โครงสร้าง UI และรายการปรับปรุง (Backlog)
│   └── ROADMAP.md                   # แผนการพัฒนาโครงการระยะถัดไป
├── public/                          # รากของเว็บไซต์ (Static Web Root)
│   ├── index.html                   # หน้าเว็บไซต์หลัก (Landing Page, Live Status, Showcase)
│   ├── data/
│   │   └── showcases.toml           # ข้อมูลผลงานในส่วน Community Showcase (แก้ไขผ่าน PR ได้)
│   ├── discord/
│   │   └── index.html               # หน้า Redirect ไปยังชุมชน Discord (/discord)
│   ├── images/
│   │   ├── favicon/                 # ไอคอนเว็บไซต์และ Web App Manifest
│   │   ├── logo/                    # โลโก้หน่วยงานพันธมิตร (วุฒิสภา, สลค., กฤษฎีกา, DGA, UpPass, iApp)
│   │   └── og/                      # รูปภาพ Open Graph สำหรับแชร์ลงโซเชียลมีเดีย
│   └── press/
│       └── 2025-12-31_blognone/     # บทความและภาพประกอบเบื้องหลังการพัฒนาโครงการ (Blognone)
├── AGENTS.md                        # คู่มือหลักสำหรับ AI Coding Agents และนักพัฒนา
├── CLAUDE.md                        # ทางลัดบริบทโปรเจกต์สำหรับ Claude Code
├── GEMINI.md                        # ทางลัดบริบทโปรเจกต์สำหรับ Gemini / Antigravity
├── CHANGELOG.md                     # ประวัติการเปลี่ยนแปลงของโปรเจกต์
├── CODE_OF_CONDUCT.md               # ข้อตกลงการอยู่ร่วมกันของชุมชนนักพัฒนา
├── CONTRIBUTING.md                  # แนวทางการมีส่วนร่วมและการส่งผลงานเข้า Community Showcase
├── SECURITY.md                      # นโยบายความปลอดภัยและการแจ้งช่องโหว่
├── CNAME                            # โดเมนสำหรับ GitHub Pages (web.openlawdatathailand.org)
└── README.md                        # เอกสารแนะนำโครงการฉบับนี้
```

---

## 💻 การรันเว็บไซต์ในเครื่อง (Local Development)

เนื่องจากหน้าเว็บมีการใช้ `fetch('./data/showcases.toml')` เพื่อโหลดรายการ Community Showcase การเปิดไฟล์ `public/index.html` แบบ `file://` โดยตรงจะติดข้อจำกัดด้าน CORS ของเบราว์เซอร์ แนะนำให้รันผ่าน Local HTTP Server:

```bash
# วิธีที่ 1: ใช้ Python 3 (แนะนำ ไม่ต้องติดตั้งแพ็กเกจเพิ่ม)
python3 -m http.server 8000 --directory public

# วิธีที่ 2: ใช้ Node.js (npx)
npx serve public -l 8000
```

จากนั้นเปิดเบราว์เซอร์ไปที่ `http://localhost:8000`

ดูรายละเอียดการแก้ไขโค้ด การเพิ่มแหล่งข้อมูลใหม่ และการปรับแต่ง UI ได้ที่ **[`docs/DEVELOPMENT.md`](docs/DEVELOPMENT.md)**

---

## 🤝 การมีส่วนร่วม (Contributing)

เรายินดีต้อนรับทุกการมีส่วนร่วม ไม่ว่าจะเป็นการส่งผลงานเข้า **Community Showcase** การแจ้งปัญหา หรือการช่วยพัฒนาหน้าเว็บไซต์:

1. **เพิ่มผลงานของคุณใน Community Showcase**:
   - แก้ไขไฟล์ [`public/data/showcases.toml`](public/data/showcases.toml) และเปิด Pull Request
   - ดูขั้นตอนอย่างละเอียดได้ที่ **[`CONTRIBUTING.md`](CONTRIBUTING.md)**
2. **ร่วมพัฒนาเว็บไซต์และเอกสาร**:
   - ศึกษาสถาปัตยกรรมและแนวทางการพัฒนาได้ที่ [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) และ [`docs/DEVELOPMENT.md`](docs/DEVELOPMENT.md)
3. **พูดคุยแลกเปลี่ยนในชุมชน**:
   - เข้าร่วม **[Discord Open Law Data](https://www.openlawdatathailand.org/discord)**

---

## 🏛️ พันธมิตรและผู้สนับสนุน

* **ผู้ริเริ่มและขับเคลื่อนโครงการ**: คณะกรรมาธิการการพาณิชย์และการอุตสาหกรรม วุฒิสภา (คณะทำงาน Open Law Data โดย สว.ตวงคุณ ทรงธรรมวัฒน์)
* **พันธมิตรภาครัฐผู้สนับสนุนข้อมูลและโครงสร้างพื้นฐาน**:
  * สำนักเลขาธิการคณะรัฐมนตรี (สลค.) — ข้อมูลราชกิจจานุเบกษา
  * สำนักงานคณะกรรมการกฤษฎีกา (OCS) — ข้อมูลประมวลกฎหมาย
  * สำนักงานพัฒนารัฐบาลดิจิทัล (องค์การมหาชน) หรือ สพร. (DGA) — โครงสร้างพื้นฐานดิจิทัล
* **พันธมิตรภาคเอกชนผู้สนับสนุนเทคโนโลยี**:
  * **UpPass** — สนับสนุนเทคโนโลยีและการออกแบบระบบ Data Pipeline
  * **iApp Technology** — สนับสนุนเทคโนโลยี AI OCR ภาษาไทย
  * **สภาอุตสาหกรรมจังหวัดสมุทรสงคราม** — สนับสนุนอุปกรณ์ประมวลผล Synology NAS
  * **StockRadars** — สนับสนุนเครือข่ายความร่วมมือและการแสดงผลข้อมูล

---

## 📚 เอกสารเพิ่มเติม (Documentation Index)

* [สถาปัตยกรรมระบบ (`docs/ARCHITECTURE.md`)](docs/ARCHITECTURE.md)
* [โครงสร้างข้อมูลและ Data Dictionary (`docs/DATA_DICTIONARY.md`)](docs/DATA_DICTIONARY.md)
* [คู่มือสำหรับนักพัฒนา (`docs/DEVELOPMENT.md`)](docs/DEVELOPMENT.md)
* [แผนการพัฒนาโครงการ (`docs/ROADMAP.md`)](docs/ROADMAP.md)
* [เบื้องหลังการพัฒนาฉบับเต็ม (`public/press/2025-12-31_blognone/content.md`)](public/press/2025-12-31_blognone/content.md)
* [ประวัติการอัปเดต (`CHANGELOG.md`)](CHANGELOG.md)
