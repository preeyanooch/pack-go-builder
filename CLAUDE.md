# Pack GO Builder · คู่มือทีม

Pack GO Builder (CJX New Store) คือเว็บแอปหน้าเดียวสำหรับวางแผนก่อนเปิดสาขาใหม่
ใช้กรอกข้อมูลสาขา แบ่งโซนประชาสัมพันธ์ ทำแผน PR รายวัน กำหนดจุดติดป้าย (ตั้งวันเป็น D+4 อัตโนมัติ)
ปักจุดบนผัง Layout ใส่ข้อมูลโรงแรม/ที่จอดรถ ทำสรุปให้ MKT + จัดซื้อ และสร้างสไลด์ Pack GO

โปรเจคนี้ใช้กติกาของ WorkOS ที่ `../CLAUDE.md` ด้วย (MEMORY.md, Context/, ห้ามแก้ Context/ ของบริษัท)

## โครงสร้าง

| ไฟล์ | คืออะไร |
|---|---|
| `index.html` | front end ทั้งหมด (ประมาณ 250KB): หน้าเข้าสู่ระบบ, ฟอร์มสาขา, PR, ป้าย, Layout composer, JSON/PPT, cache แบบ stale-while-revalidate + สำรองข้อมูลไว้ในเครื่อง |
| `backend/Code.gs` | Google Apps Script: doGet/doPost, save/load/list/delete draft, เก็บรูปแบบแบ่งชิ้น, checkLogin (access sheet), getBranchMasterData, createPackGoSlides |
| Vercel proxy | front end เรียกผ่าน `WEBAPP_URL` (`.../api/gsrun`) ตัวโค้ด proxy ไม่ได้อยู่ใน repo นี้ |

## ทีมประจำโปรเจค: ส่งงานตามเลน

| งาน | ส่งให้ |
|---|---|
| เขียน/แก้/รีวิวโค้ด, debug, Apps Script, Vercel/Supabase, UI/UX, สไลด์ที่สร้างอัตโนมัติ, อธิบายเรื่องเทคให้เข้าใจง่าย | **Kiki** (`nsa-store-performance:tech-engineer`) |
| วันเปิดสาขา/D-day/วัน D+4 ของป้ายตรงกับความจริงไหม, ใครดูแลสาขา, สาขาหายไปหรือค้างใน roster, ข้อมูล branch master ถูกไหม | **Coco** (`nsa-store-performance:data-integrity-tracker`) |
| ตัวเลขยอดขาย/สมาชิกเทียบเป้า D1–D5 (1,000,000 บาท + สมาชิกใหม่ 600 คน) หรือ D6–D90 (active member 4,000 คน), แผน PR ช่วยให้ถึงเป้าไหม | **Lala** (`nsa-store-performance:performance-analyst`) |
| โจทย์ข้ามเลน, ไม่ชัดว่าเป็นของใคร, หรือต้องการสรุปรวม | **Winky** (`nsa-store-performance:team-lead`) |

กติกาการส่งงาน
- โจทย์เข้าเลนเดียว ส่งตรงไปที่คนนั้น ไม่ต้องผ่าน Winky
- งานสร้างหรือแก้หน้าที่ต้องโชว์ตัวเลขสาขา ให้ Lala/Coco ยืนยันตัวเลขก่อน แล้ว Kiki ค่อยสร้าง
- Kiki ห้ามใส่ตัวเลข performance หรือวันเปิดสาขาลงโค้ดหรือข้อมูลตัวอย่างเอง ต้องมาจาก Lala/Coco หรือแหล่งข้อมูลจริงเท่านั้น
- เรียกชื่อได้โดยตรง: "คิกิ ..." / "โคโค่ ..." / "ลาล่า ..." / "วิงกี้ ..."

## กติกาโค้ด (Kiki)

- ต้องโหลด skill `nsa-store-performance:tech-build-review` ก่อนแตะโค้ด
- commit, push, deploy Apps Script, deploy Vercel และแก้ Supabase ต้องได้รับคำสั่งจากผู้ใช้ก่อนทุกครั้ง
- ห้ามเปิด ห้ามอ่าน ห้ามพิมพ์ซ้ำ `.env`, API key, token หรือรหัสผ่าน ถ้าเจอในโค้ดให้แจ้งผู้ใช้และให้ IT Security เปลี่ยน key
- ระวังจุดที่เคยพังมาแล้ว: ห้าม autosave ระหว่างที่โหลด draft, โหลดรูปไม่สำเร็จต้องแยกให้ออกจากกรณีที่ไม่มีรูปจริง, ห้าม retry การสร้างสไลด์ตอนเน็ตสะดุด
- ก่อนแก้ `backend/Code.gs` ให้ดู `git status` ก่อน อาจมีงานที่ยังไม่ได้ commit ค้างอยู่
- สรุปงานเป็นภาษาง่าย พร้อมบอกวิธีวางโค้ดและวิธีเทส

## ข้อมูลและความอ่อนไหว

- ในแอปมีชื่อ ตำแหน่ง และผู้ติดต่อของพนักงาน ใช้ในงานภายในได้ แต่ห้ามนำไปใส่ในไฟล์ตัวอย่างหรือ commit message
- วันเปิดสาขาที่ยังไม่ประกาศถือเป็น 🔴 ให้แชร์เฉพาะคนในทีม
- ทุกคำตอบเป็นฉบับร่าง ต้องมีคนตรวจก่อนนำไปใช้จริง
- ก่อนขยายให้คนใช้มากขึ้น ให้ทำ `cjx-toolkit:ai-app-screening`
