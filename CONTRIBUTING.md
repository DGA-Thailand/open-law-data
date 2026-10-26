# แนวทางการมีส่วนร่วม (Contributing to Open Law Data Thailand)

ขอบคุณที่สนใจร่วมเป็นส่วนหนึ่งในการขับเคลื่อนโครงการ **Open Law Data Thailand**!  
Thank you for your interest in contributing to the Open Law Data Thailand project. We welcome contributions in both **Thai** and **English**.

---

## 🚀 1. การส่งผลงานเข้า Community Showcase (Adding Your Project to Showcase)

หากคุณได้นำข้อมูลจาก Open Law Data ไปพัฒนาต่อยอดเป็นเครื่องมือ เว็บไซต์ สมุดบันทึกวิเคราะห์ข้อมูล (Notebook) ระบบ AI หรืองานวิจัย คุณสามารถเพิ่มผลงานของคุณขึ้นแสดงบนหน้าเว็บไซต์หลักได้ทันทีผ่าน Pull Request:

### ขั้นตอนการส่งผลงาน (Step-by-Step)

1. **Fork** คลังข้อมูลนี้ไปยังบัญชี GitHub ของคุณ
2. แก้ไขไฟล์ [`public/data/showcases.toml`](public/data/showcases.toml)
3. เพิ่มข้อมูลโปรเจกต์ของคุณต่อท้ายในรายการ `[[projects]]` ตามรูปแบบด้านล่าง:

```toml
[[projects]]
title = "ชื่อผลงานของคุณ (My Awesome Legal Tech Project)"
author = "ชื่อผู้พัฒนา หรือ ชื่อทีม/องค์กร"
author_url = "https://github.com/yourname"
description = "คำอธิบายสั้นๆ ว่าผลงานนี้ทำอะไร และใช้ข้อมูลอย่างไร (แนะนำไม่เกิน 200 ตัวอักษร)"
tags = ["AI", "Search", "Data Visualization"]
url = "https://your-project-url.com"
featured = false
date = "YYYY-MM-DD"
```

### คำอธิบายฟิลด์ (Field Specifications)

| ฟิลด์ (Field) | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
| :--- | :--- | :---: | :--- |
| `title` | `String` | ✅ | ชื่อโปรเจกต์หรือผลงาน |
| `author` | `String` | ✅ | ชื่อผู้พัฒนา ชื่อทีม หรือองค์กร |
| `author_url` | `String (URL)` | ✅ | ลิงก์โปรไฟล์ GitHub หรือเว็บไซต์ของผู้พัฒนา (ต้องขึ้นต้นด้วย `https://` หรือ `http://`) |
| `description` | `String` | ✅ | คำอธิบายสั้นๆ เกี่ยวกับผลงาน (ความยาวไม่เกิน ~200 ตัวอักษร เพื่อให้แสดงผลในการ์ดได้สวยงาม) |
| `tags` | `Array<String>` | ✅ | หมวดหมู่หรือเทคโนโลยีที่ใช้ เช่น `["BigQuery", "SQL"]`, `["Python", "Google Colab"]`, `["RAG", "LLM"]` (แนะนำ 2–4 แท็ก) |
| `url` | `String (URL)` | ✅ | ลิงก์ไปยังหน้าเว็บใช้งานจริง, GitHub Repository, หรือ Colab Notebook (ต้องขึ้นต้นด้วย `https://`) |
| `featured` | `Boolean` | ➖ | กำหนดเป็น `false` เสมอเมื่อส่ง PR (ผู้ดูแลโครงการจะพิจารณาปรับเป็น `true` สำหรับผลงานเด่น) |
| `date` | `String (Date)` | ✅ | วันที่เปิดตัวหรือวันที่ส่งผลงาน ในรูปแบบ ISO 8601 (`YYYY-MM-DD`) |

4. **ทดสอบการแสดงผลในเครื่อง (ทางเลือก)**:
   รันคำสั่ง `python3 -m http.server 8000 --directory public` แล้วเปิด `http://localhost:8000/#showcase` เพื่อตรวจสอบว่าการ์ดของคุณแสดงผลถูกต้องและไฟล์ TOML ไม่มี Syntax Error
5. เปิด **Pull Request** มายังกิ่ง `main`

---

## 🛠️ 2. การมีส่วนร่วมพัฒนาเว็บไซต์และเอกสาร (Website & Documentation Contributions)

คลังข้อมูลนี้ออกแบบด้วยสถาปัตยกรรม **Stateless Static Web** (HTML5 + Tailwind CSS CDN + Vanilla JavaScript) เพื่อให้ดูแลรักษาง่าย รวดเร็ว และไม่มีภาระค่าใช้จ่ายเซิร์ฟเวอร์

### แนวทางการพัฒนา (Development Guidelines)

* **ไม่ต้องมีขั้นตอน Build**: ไฟล์ทั้งหมดใน `public/` จะถูกนำไปเผยแพร่ขึ้น GitHub Pages โดยตรงผ่าน `.github/workflows/static.yml`
* **การทดสอบในเครื่อง**:
  ```bash
  python3 -m http.server 8000 --directory public
  ```
* **การออกแบบและสไตล์ (Design & Styling)**:
  * ใช้ฟอนต์ `Prompt` สำหรับภาษาไทยและอังกฤษ
  * ใช้สีหลักของโครงการที่กำหนดไว้ใน Tailwind config (`thai-blue`: `#1e3a8a`, `thai-dark`: `#0f172a`, `tech-accent`: `#3b82f6`)
  * ใช้ไอคอนจาก [Phosphor Icons](https://phosphoricons.com/) (`ph-*`, `ph-fill`, `ph-bold`)
* **ข้อควรระวังเมื่อใช้ Tailwind CDN กับ JavaScript**:
  * หลีกเลี่ยงการต่อสตริงชื่อคลาสแบบไดนามิก (เช่น ``bg-${color}-50``) โดยไม่มีคลาสนั้นปรากฏอยู่ใน HTML หรือ Safelist หากต้องเพิ่มสีใหม่ใน `sources[]` ควรตรวจสอบว่าคลาสสีดังกล่าวถูกสแกนโดย Tailwind CDN แล้ว
* **ความปลอดภัย (Security)**:
  * ห้ามใส่ลิงก์ที่เป็น `javascript:` หรือโค้ด HTML ดิบลงใน `public/data/showcases.toml`
  * ดูรายละเอียดเพิ่มเติมใน [`docs/DEVELOPMENT.md`](docs/DEVELOPMENT.md) และ [`SECURITY.md`](SECURITY.md)

### ขั้นตอนการส่ง Pull Request สำหรับการแก้ไขโค้ด

1. สร้าง Branch ใหม่จาก `main` (เช่น `feat/add-ocs-source`, `fix/mobile-nav-links`, `docs/update-guide`)
2. ใช้ข้อความ Commit ตามแนวทาง Conventional Commits:
   * `feat:` สำหรับฟีเจอร์หรือส่วนแสดงผลใหม่
   * `fix:` สำหรับการแก้ไขบั๊กหรือการแสดงผลผิดพลาด
   * `docs:` สำหรับการแก้ไขเอกสาร `.md`
   * `chore:` สำหรับงานดูแลรักษาทั่วไปหรืออัปเดตข้อมูล
3. ตรวจสอบการแสดงผลทั้งบนหน้าจอ Desktop และ Mobile
4. เปิด Pull Request พร้อมระบุสิ่งที่เปลี่ยนแปลงอย่างชัดเจน

---

## 💬 ช่องทางพูดคุยและสอบถาม

หากมีข้อสงสัยหรือต้องการหารือแนวทางการพัฒนา สามารถพูดคุยกับทีมงานและชุมชนนักพัฒนาได้ที่ **[Discord Open Law Data Thailand](https://www.openlawdatathailand.org/discord)**
