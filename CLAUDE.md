# มาม่าซัง — ระบบสั่งอาหารร้านบุฟเฟต์

## Tech stack
- Next.js เวอร์ชันล่าสุด (App Router) — **JavaScript เท่านั้น ไม่ใช้ TypeScript**
- Supabase (ใช้ผ่าน `lib/supabaseClient.js`)
- Deploy บน Vercel

## กฎสำคัญ: Dynamic Route params เป็น Promise
ใน Next.js เวอร์ชันนี้ `params` (และ `searchParams`) ของ Dynamic Route เป็น **Promise**
ห้ามอ่านค่าตรง ๆ เช่น `params.tableId` ต้อง unwrap ก่อนเสมอ

**Client Component** (ใช้ `use()` จาก React):
```js
"use client";
import { use } from "react";

export default function OrderPage({ params }) {
  const { sessionId } = use(params);
  // ...
}
```

**Server Component** (ไม่มี "use client"): ใช้ `const { sessionId } = await params;` ในฟังก์ชัน async

หน้าสั่งอาหารในขั้นตอนถัดไปจะเป็น Client Component จึงต้องใช้ `use(params)` เสมอ

## Environment variables
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`

ห้าม commit `.env.local` — ดูตัวอย่างที่ `.env.example`

## โครงสร้างฐานข้อมูล (มีอยู่แล้วใน Supabase — ไม่ต้องสร้างใหม่)
ใช้ชื่อตารางและคอลัมน์ตามนี้เท่านั้น ห้ามเดาคอลัมน์เพิ่ม

### sessions
| คอลัมน์ | หมายเหตุ |
|---|---|
| id | |
| table_number | เลขโต๊ะ |
| adult_count | จำนวนผู้ใหญ่ |
| child_count | จำนวนเด็ก |
| status | สถานะ session |
| created_at | |

### menu_categories
| คอลัมน์ | หมายเหตุ |
|---|---|
| id | |
| name | ชื่อหมวดหมู่ |
| sort_order | ลำดับการแสดงผล |

### menu_items
| คอลัมน์ | หมายเหตุ |
|---|---|
| id | |
| category_id | อ้างอิง menu_categories.id |
| name | ชื่อเมนู |

### orders
| คอลัมน์ | หมายเหตุ |
|---|---|
| id | |
| session_id | อ้างอิง sessions.id |
| table_number | |
| items | jsonb — รายการอาหารที่สั่ง |
| status | สถานะออเดอร์ |
| created_at | |

## หน้าที่วางแผนไว้
- `/` — หน้าแรก (ใช้ทดสอบ deploy)
- `/generate-qr` — สร้าง QR Code ประจำโต๊ะ
- `/kitchen` — หน้าครัว
- หน้าสั่งอาหาร (Dynamic Route) — ขั้นตอนถัดไป
