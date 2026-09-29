# มาม่าซัง

ระบบสั่งอาหารร้านบุฟเฟต์ — Next.js (App Router, JavaScript) + Supabase, deploy บน Vercel

## เริ่มใช้งาน (local)
```bash
npm install
cp .env.example .env.local   # แล้วใส่ค่าจริง
npm run dev
```

## Deploy บน Vercel
1. Import repo จาก GitHub
2. ตั้ง Environment Variables: `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`
3. Deploy

> ดูกฎและโครงสร้างฐานข้อมูลของโปรเจกต์ที่ `CLAUDE.md`
> (สำคัญ: `params` ของ Dynamic Route เป็น Promise ต้อง unwrap ด้วย `use()`)
