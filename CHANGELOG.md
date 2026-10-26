# Changelog

ประวัติการเปลี่ยนแปลงที่สำคัญทั้งหมดของโปรเจกต์ **Open Law Data Thailand Web Portal (`open-law-data-web`)** ถูกรวบรวมไว้ในไฟล์นี้  
รูปแบบอ้างอิงตามมาตรฐาน [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)

---

## [2026-10-27] — ยกระดับเว็บไซต์ โครงสร้างเอกสาร และเพิ่ม Thai Legal Watch

### Added
- เพิ่ม **Thai Legal Watch** เข้าสู่ส่วน Community Showcase ใน [`public/data/showcases.toml`](public/data/showcases.toml) (`f5015c8` / PR `#1`)
- เพิ่ม Interactive Tabs (`meta/`, `ocr/`, `hf cli`) และปุ่มคัดลอกคำสั่งในส่วน **โครงสร้างข้อมูล (`#data-preview`)** ของ [`public/index.html`](public/index.html)
- เพิ่มการ์ดสถานะ `coming_soon` สำหรับข้อมูล **ประมวลกฎหมาย (สำนักงานคณะกรรมการกฤษฎีกา)** ในหน้าต่าง `#update-popup`
- เพิ่มชุดเอกสารโครงสร้างโปรเจกต์และคู่มือนักพัฒนา:
  - [`AGENTS.md`](AGENTS.md), [`GEMINI.md`](GEMINI.md), [`CLAUDE.md`](CLAUDE.md) สำหรับเป็นบริบทให้ AI Coding Assistants และนักพัฒนา
  - [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) อธิบายสถาปัตยกรรมระบบ Data Pipeline และ Stateless Web Portal
  - [`docs/DATA_DICTIONARY.md`](docs/DATA_DICTIONARY.md) รวบรวมโครงสร้างข้อมูลบน Hugging Face, `latest_update.json` และ `showcases.toml`
  - [`docs/DEVELOPMENT.md`](docs/DEVELOPMENT.md) คู่มือการรันเว็บไซต์ในเครื่อง มาตรฐาน UI และรายการตรวจสอบทางเทคนิค (Technical Audit)
  - [`docs/ROADMAP.md`](docs/ROADMAP.md) แผนการพัฒนาโครงการระยะถัดไป
  - [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md), [`SECURITY.md`](SECURITY.md), [`CHANGELOG.md`](CHANGELOG.md) และ [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md)

### Changed
- ปรับปรุงแถบเมนู Navigation ทั้ง Desktop และ Mobile ให้มีลิงก์ครบทุกส่วนสำคัญ (`#stats`, `#showcase`, `#about`, `#architecture`, `#partners`, `#data-preview`) และพับเก็บเมนูมือถืออัตโนมัติเมื่อกดลิงก์
- ปรับปรุง [`README.md`](README.md) และ [`CONTRIBUTING.md`](CONTRIBUTING.md) ให้ครอบคลุมทั้งการส่งผลงาน Showcase และการพัฒนาเว็บไซต์
- ขยายการแสดงผลคำอธิบายการ์ดใน `#showcase` เป็น `line-clamp-4` พร้อม `title` attribute

### Fixed
- แก้ไขแท็ก `<p>` ที่ตกหล่นในส่วน `#credits` และลบบล็อก `hidden` ที่ซ้ำซ้อนใน `#partners`
- แก้ไข URL ของ `og:image` ทั้งใน `public/index.html` และ `public/discord/index.html` ให้เป็น Absolute URL (`https://www.openlawdatathailand.org/images/og/og_image.png`)
- ปรับข้อความสัญญาอนุญาตใน Footer ให้ตรงกับชุดข้อมูล (`CC BY 4.0 (Data) · MIT (Code)`) และอัปเดตปีลิขสิทธิ์เป็น `2568–2569`
- เพิ่ม `escapeHtml()`, `safeUrl()`, Static Tailwind `colorStyles` Map และการกดปุ่ม `Escape` เพื่อปิดหน้าต่าง `#update-popup`

---

## [2026-02-09] — พันธมิตรภาครัฐและโลโก้หน่วยงาน

### Added
- เพิ่ม **สำนักงานคณะกรรมการกฤษฎีกา (Office of the Council of State - OCS)** ในส่วนพันธมิตรและผู้สนับสนุนข้อมูลประมวลกฎหมาย (`9e526de`)
- เพิ่มโลโก้ทางการของหน่วยงานพันธมิตรใน `public/images/logo/`:
  - คณะกรรมาธิการการพาณิชย์และการอุตสาหกรรม วุฒิสภา (`senate.jpg`, `5ba8e74`)
  - สำนักเลขาธิการคณะรัฐมนตรี (`soc.jpg`, `4591ef4`)
  - สำนักงานคณะกรรมการกฤษฎีกา (`ocs.png`, `4591ef4`)
  - สำนักงานพัฒนารัฐบาลดิจิทัล (`dga.png`, `4591ef4`)

---

## [2026-01-19] — ระบบ Community Showcase

### Added
- เปิดตัวส่วน **Community Showcase (`#showcase`)** แสดงผลงานที่พัฒนาต่อยอดโดยชุมชนนักพัฒนาไทย (`7449734`)
- รองรับการโหลดข้อมูลโปรเจกต์แบบ Dynamic จากไฟล์ [`public/data/showcases.toml`](public/data/showcases.toml) ด้วยไลบรารี `fast-toml`
- เพิ่มผลงานชุดแรกใน `showcases.toml` (`235a6d3`):
  - *Open Law Data on BigQuery* โดย `dtinth`
  - *Ratchakitcha Data on Colab* โดย `spicydog`
- เพิ่มไฟล์ [`CONTRIBUTING.md`](CONTRIBUTING.md) แนะนำขั้นตอนการส่ง Pull Request เข้า Showcase

### Changed
- ปรับหัวข้อแต่ละส่วนใน `public/index.html` ให้ใช้ภาษาไทยเป็นหลัก (`e490b67`)
- อัปเดต `README.md` ให้สะท้อนสถานะการเปิดใช้งานจริงและเพิ่มลิงก์เข้าร่วม Discord (`51d88aa`, `89561c6`)

---

## [2026-01-02] — ชุมชน Discord และบทความเบื้องหลังการพัฒนา (Press)

### Added
- เพิ่มหน้า Redirect ไปยังชุมชน Discord ที่ `public/discord/index.html` (`/discord`) พร้อมลิงก์ในส่วน Footer (`bb25edd`, `867a194`)
- เพิ่มบทความและภาพประกอบเบื้องหลังการพัฒนาโครงการสำหรับเผยแพร่บน Blognone ใน `public/press/2025-12-31_blognone/` (`f6a81a4`–`3d1fe52`)
- เพิ่มรูปภาพ Open Graph (`public/images/og/og_image.png`) สำหรับการแชร์ลิงก์บนโซเชียลมีเดีย (`efa0a52`–`674f860`)

---

## [2025-12-09] — เครดิตผู้สนับสนุนเทคโนโลยีและระบบติดตามสถานะอัปเดต

### Added
- เพิ่มส่วน **เทคโนโลยีเบื้องหลัง (`#credits`)** ขอบคุณพันธมิตรเทคโนโลยี ได้แก่ **UpPass** และ **iApp Technology** (`4680ae8`)
- เชื่อมต่อระบบแสดงสถานะอัปเดตล่าสุด (`#latest-update-trigger` และ `#update-popup`) เข้ากับไฟล์ `latest_update.json` บน Hugging Face แบบ Real-time (`346237a`–`e1af2fb`)
- เพิ่มส่วนแสดงโครงสร้างข้อมูลและคำอธิบายชุดข้อมูล (`#data-preview` / Data Dictionary) (`16bac8a`)
- เพิ่มชุด Favicon และ Web App Manifest ใน `public/images/favicon/` (`373c380`, `b53fb33`)

---

## [2025-11-24] — เปิดตัวเว็บไซต์หลักและข้อมูล OCR ราชกิจจานุเบกษา

### Added
- เปิดตัวหน้าเว็บไซต์หลัก `public/index.html` พร้อมระบบอัปโหลดและ Deploy อัตโนมัติขึ้น GitHub Pages ผ่าน `.github/workflows/static.yml` (`b731ac9`–`d032dcc`)
- ตั้งค่า Custom Domain `web.openlawdatathailand.org` ในไฟล์ `CNAME` (`07c09cf`)
- เริ่มต้นสร้างคลังข้อมูลและ `README.md` (`1c244c0`, 2025-09-02)
