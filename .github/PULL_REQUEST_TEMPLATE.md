## รายละเอียดการเปลี่ยนแปลง (What & Why)

<!-- อธิบายสั้นๆ ว่า Pull Request นี้เพิ่มหรือแก้ไขอะไร และเพราะเหตุใด -->

## ประเภทของงาน (Type of Change)
- [ ] 🚀 **Community Showcase**: เพิ่มหรืออัปเดตผลงานใน `public/data/showcases.toml`
- [ ] ✨ **Feature / Content**: เพิ่มฟีเจอร์ใหม่ แหล่งข้อมูลใหม่ หรือข้อมูลพันธมิตรในหน้าเว็บไซต์
- [ ] 🐛 **Bug Fix**: แก้ไขการแสดงผล UI, ลิงก์เสีย หรือสคริปต์
- [ ] 📚 **Documentation / CI**: แก้ไขไฟล์เอกสาร `.md` หรือ GitHub Actions

## รายการตรวจสอบ (Checklist)

### กรณีเพิ่มผลงานใน `public/data/showcases.toml`
- [ ] ระบุฟิลด์ครบถ้วน (`title`, `author`, `author_url`, `description`, `tags`, `url`, `featured = false`, `date = "YYYY-MM-DD"`)
- [ ] ลิงก์ `url` และ `author_url` ใช้งานได้จริงและขึ้นต้นด้วย `https://` หรือ `http://`
- [ ] รูปแบบไฟล์ TOML ถูกต้อง (ไม่มี Syntax Error)

### กรณีแก้ไขโค้ดเว็บไซต์ (`public/`)
- [ ] ทดสอบรันผ่าน Local HTTP Server (`python3 -m http.server 8000 --directory public`) แล้วแสดงผลได้ถูกต้อง
- [ ] ตรวจสอบการแสดงผลทั้งบนหน้าจอ Desktop และ Mobile
- [ ] อัปเดตเอกสารใน `docs/` หรือ `CHANGELOG.md` หากมีการเปลี่ยนแปลงโครงสร้างสำคัญ
