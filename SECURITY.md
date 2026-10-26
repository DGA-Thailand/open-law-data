# นโยบายความมั่นคงปลอดภัย (Security Policy)

## การแจ้งช่องโหว่ด้านความปลอดภัย (Reporting a Vulnerability)

หากท่านพบช่องโหว่ด้านความปลอดภัยในเว็บไซต์หรือระบบของ **Open Law Data Thailand** กรุณา**หลีกเลี่ยงการเปิด Public Issue** บน GitHub โดยตรง

* กรุณาแจ้งผ่านฟีเจอร์ **[Private Vulnerability Reporting](../../security/advisories/new)** ของคลังข้อมูลนี้บน GitHub หรือติดต่อทีมผู้ดูแลโครงการโดยตรงผ่าน [Discord ของโครงการ](https://www.openlawdatathailand.org/discord)
* ทีมงานจะตรวจสอบและตอบกลับภายใน 3–5 วันทำการ

---

## ขอบเขตการดูแลความปลอดภัย (Scope)

1. **เว็บไซต์หน้าบ้าน (`public/`)**:
   * การป้องกันช่องโหว่ Cross-Site Scripting (XSS) หรือการฝังลิงก์อันตราย (`javascript:`, `data:`) ผ่านการส่ง Pull Request ในไฟล์ [`public/data/showcases.toml`](public/data/showcases.toml)
   * ความปลอดภัยของทรัพยากรภายนอก (CDN Scripts, Subresource Integrity, Open Redirect)
2. **ระบบ CI/CD (`.github/workflows/static.yml`)**:
   * การจำกัดสิทธิ์ของ `GITHUB_TOKEN` ตามหลัก Least Privilege (`contents: read`, `pages: write`, `id-token: write`)
3. **ความถูกต้องของข้อมูล (Data Integrity)**:
   * การตรวจสอบความถูกต้องของลิงก์ที่ชี้ไปยังชุดข้อมูลทางการบน Hugging Face (`open-law-data-thailand`)

---

## แนวปฏิบัติที่ดีสำหรับผู้ดูแลและผู้ร่วมพัฒนา (Security Best Practices)

* **ไม่มีการเก็บความลับในคลังข้อมูลนี้ (Zero Secrets in Repo)**: คลังข้อมูลนี้เป็น Static Website ล้วน ห้ามเพิ่ม API Keys, Tokens หรือข้อมูลส่วนบุคคล (PII) ลงในโค้ดหรือประวัติ Git โดยเด็ดขาด
* **การตรวจสอบ PR สำหรับ `showcases.toml`**:
  * ตรวจสอบว่าค่า `url` และ `author_url` เป็น URL ที่ปลอดภัย (`https://` หรือ `http://`)
  * ตรวจสอบว่าไม่มีโค้ด HTML หรือ `<script>` แฝงอยู่ในฟิลด์ `title`, `description`, `author`, หรือ `tags`
