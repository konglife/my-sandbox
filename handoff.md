# Handoff — `kong-sandbox`

เอกสารส่งต่องานสำหรับเปิดแชทใหม่ (New Conversation) ในโปรเจ็กต์ Sandbox นี้

---

## 1. เป้าหมายของโปรเจ็กต์
- พื้นที่ทดลองทั่วไป (General-purpose Cloud Sandbox) สำหรับทดสอบไอเดีย สคริปต์ เครื่องมือ Web UI/API และการทำงานร่วมกันระหว่าง GitHub กับ Google Drive

## 2. สเปกและข้อจำกัดของ Cloud VM (Remote Agents)
- **OS:** Ubuntu 24.04 Linux (`x86_64`, user `antigravity`, home `/home/antigravity`)
- **CPU / RAM / Disk:** Intel Xeon @ 2.30GHz (**2 vCPUs**), RAM **7.8 GiB** (+ Swap 8 GiB), พื้นที่ว่าง ~85 GB
- **GPU:** **ไม่มี (CPU-only)**
  - *เหมาะกับ:* Web Apps, APIs, Python / Node.js / Go / Rust, Data analysis, Automation, Generative UI (`.html` artifacts) และการจัดการไฟล์ผ่าน GitHub / Google Drive
  - *ไม่เหมาะกับ:* งานเรนเดอร์วิดีโอทีละเฟรมด้วย WebGL/3D หนักๆ หรือรันโมเดล Local ML ที่ต้องพึ่ง GPU

## 3. ทรัพยากรที่เชื่อมต่อไว้ (Connected Resources)

| ประเภท | ชื่อ | ตำแหน่ง / ID | หมายเหตุการใช้งาน |
|---|---|---|---|
| **GitHub Repo** | `konglife/my-sandbox` | `/home/antigravity/konglife/my-sandbox` | Public repo สำหรับเก็บโค้ดและไฟล์ที่ต้องการให้ซิงก์ข้ามแชทผ่าน Git |
| **Google Drive Project** | `kong-sandbox` | `1uGZTWH8nj1a6s93fMfUIXdHSAeSnhQQ` (`$SANDBOX_DRIVE_PROJECT_ID`) | ชนิด `google-apps.project` (Virtual AI Workspace) ใช้รวมบริบทให้ Gemini ใน Drive **อัปโหลดไฟล์ใส่เป็นลูกโดยตรงไม่ได้ (`canAddChildren: false`)** |
| **Google Drive Folder** | `kong-sandbox-files` | `1RZ30VW5RxOTFml3Unfx0Q5gn8uBHT-Ci` (`$SANDBOX_DRIVE_FILES_FOLDER_ID`) | โฟลเดอร์ปกติใน `My Drive / DEV / kong-sandbox-files` **ใช้ ID นี้กับ `gdrive mutate upload --parent` เวลาเก็บไฟล์ถาวรขึ้น Drive** |

*(หมายเหตุ: ในโฟลเดอร์ `kong-sandbox-files/homo-sapiens-migration` บน Google Drive มีไฟล์วิดีโอทดลองจากเซสชันแรกเก็บไว้ เผื่อเปิดดูหรือลบออกได้ตามต้องการ)*

## 4. การตั้งค่าอัตโนมัติ (`env-setup.sh`)
- ตั้งค่า Project-scoped `/home/antigravity/env-setup.sh` ไว้แล้ว ระบบจะรันอัตโนมัติทุกครั้งที่บูต VM ของแชทใหม่ในโปรเจ็กต์นี้:
  - เปิดใช้งาน `~/.bashrc` สำหรับ non-interactive shell และเพิ่ม `~/.local/bin` เข้า `PATH`
  - ตั้งค่าตัวแปร `$SANDBOX_DRIVE_PROJECT_ID` และ `$SANDBOX_DRIVE_FILES_FOLDER_ID`
  - เตรียมความพร้อม `gh` (GitHub CLI + git credential) และ `gdrive` CLI

## 5. ทิปเริ่มงานในแชทใหม่
- แชทใหม่สามารถอ่านไฟล์ `handoff.md` นี้เพื่อรับบริบททั้งหมดแล้วเริ่มงานหัวข้อใหม่ได้ทันที
- สำหรับงานเขียนโค้ดและสั่ง Agent ทั่วไป แนะนำโมเดล **Gemini 4 Neon (High)** หรือโหมด **Orchestrator** เพื่อความเร็วและความคุ้มโควตาสูงสุด
