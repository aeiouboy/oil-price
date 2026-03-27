# PRD: Oil Price Dashboard (Bangchak)

---

## 1. Product Overview

| Field | Value |
|-------|-------|
| **Product Name** | Oil Price Dashboard |
| **Version** | 1.1 |
| **Last Updated** | 27 March 2026 |
| **Data Source** | https://oil-price.bangchak.co.th/ApiOilPrice2/th |

### 1.1 Objective

สร้าง Dashboard แสดงราคาน้ำมันจาก Bangchak API โดยแสดงข้อมูลราคาวันนี้และเมื่อวาน พร้อมกราฟเปรียบเทียบ และคำนวณระยะทางตามประเภทรถ

### 1.2 Target Users

- ผู้ใช้ทั่วไปที่ต้องการตรวจสอบราคาน้ำมัน
- เจ้าของธุรกิจขนส่ง/โลจิสติกส์
- ผู้ที่วางแผนเดินทาง

---

## 2. Features

| Feature ID | Feature Name | Priority | Status |
|------------|--------------|----------|--------|
| F-001 | แสดงราคาน้ำมันทุกประเภท (Cards) | High | Done |
| F-002 | กราฟเปรียบเทียบราคา (Bar Chart) | High | Done |
| F-003 | Widget ราคาย้อนหลัง (Today vs Yesterday) | High | Done |
| F-004 | Auto Refresh ทุก 1 นาที | Medium | Done |
| F-005 | Manual Refresh Button | Medium | Done |
| F-006 | Loading States / Skeleton | Medium | Done |
| F-007 | Error Handling | Medium | Done |
| F-008 | คำนวณระยะทางตามประเภทรถ | High | Done |

---

## 3. API Specification

### 3.1 Endpoint

```
GET https://oil-price.bangchak.co.th/ApiOilPrice2/th
```

### 3.2 Response Schema

```typescript
interface OilPriceResponse {
  OilRemark1: string      // วันที่มีผลบังคับใช้
  OilRemark2: string      // คำอธิบายเพิ่มเติม
  OilList: OilItem[]
}

interface OilItem {
  OilName: string         // ชื่อน้ำมัน
  PriceToday: number      // ราคาวันนี้
  PriceYesterday: number  // ราคาเมื่อวาน
  PriceDifYesterday: number // ส่วนต่าง (จาก API)
  IconWeb: string         // URL ไอคอน
  IconWeb3: string        // URL ไอคอน (alt)
}
```

### 3.3 Oil Types Available

- Gasohol S EVO 91
- Gasohol S EVO 95
- Gasohol S EVO E20
- Hi Premium Diesel S B7
- Hi Diesel S B7

---

## 4. Car Specifications (F-008)

| Car Type | Tank Size | Fuel Efficiency | Recommended Fuel |
|----------|-----------|-----------------|------------------|
| Eco Car | 36 L | 20 km/L | Gasohol 91, 95 |
| Sedan | 50 L | 14 km/L | Gasohol 95 |
| Pickup (กระบะ) | 76 L | 11 km/L | Diesel B7 |

### 4.1 Calculation Formulas

```
Distance = Fuel Amount (L) x Fuel Efficiency (km/L)
Cost = Fuel Amount (L) x Price per Liter (B)
Fuel from Budget = Budget (B) / Price per Liter (B)
```

---

## 5. Wireframe (ASCII)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        OIL PRICE DASHBOARD                                  │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │ [Icon] ราคาน้ำมันวันนี้                      [Refresh] [Auto]       │   │
│  │ มีผลตั้งแต่: 27 มีนาคม 2569 เวลา 05:00 น.                          │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  ┌─────────────── SECTION: OIL PRICE CARDS ───────────────────────────┐    │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐   │    │
│  │  │ Gasohol 91  │ │ Gasohol 95  │ │ Gasohol E20 │ │ Diesel B7   │   │    │
│  │  │  B35.88     │ │  B43.44     │ │  B34.74     │ │  B32.44     │   │    │
│  │  │ [+0.50]     │ │ [0.00]      │ │ [-0.30]     │ │ [0.00]      │   │    │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘   │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                             │
│  ┌─────────────── SECTION: HISTORY WIDGET ────────────────────────────┐    │
│  │  เปรียบเทียบราคา: วันนี้ vs เมื่อวาน                               │    │
│  │  [ Select Oil: Gasohol S EVO 91 v ]                                │    │
│  │       ┌─────┐   ┌─────┐                                            │    │
│  │       │Today│   │Yest │   [+0.50 บาท] ราคาเพิ่มขึ้น               │    │
│  │       └─────┘   └─────┘                                            │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                             │
│  ┌─────────────── SECTION: DISTANCE CALCULATOR ───────────────────────┐    │
│  │                                                                     │    │
│  │  [MapPin] คำนวณระยะทาง                                             │    │
│  │  ประมาณการระยะทางที่ขับได้จากจำนวนน้ำมันหรืองบประมาณ               │    │
│  │                                                                     │    │
│  │  ┌─────────────────────────────────────────────────────────────┐   │    │
│  │  │  [ Eco Car ]  [ Sedan ]  [ กระบะ ]     <-- Tab Selection    │   │    │
│  │  └─────────────────────────────────────────────────────────────┘   │    │
│  │                                                                     │    │
│  │  ┌─────────────────────────────────────────────────────────────┐   │    │
│  │  │ [Car Icon]  Eco Car                                          │   │    │
│  │  │             รถประหยัดพลังงาน เช่น March, Mirage, Brio        │   │    │
│  │  │             [ถัง 36 ลิตร]  [20 km/l]                         │   │    │
│  │  └─────────────────────────────────────────────────────────────┘   │    │
│  │                                                                     │    │
│  │  ┌─────────────────────────────────────────────────────────────┐   │    │
│  │  │ น้ำมันที่แนะนำ: Gasohol S EVO 91        ราคา: B35.88/ลิตร   │   │    │
│  │  └─────────────────────────────────────────────────────────────┘   │    │
│  │                                                                     │    │
│  │  ┌───────────────────────┐  ┌───────────────────────┐              │    │
│  │  │ จำนวนน้ำมัน (ลิตร)   │  │ งบประมาณ (บาท)       │              │    │
│  │  │ [    20    ]          │  │ [    500   ]          │              │    │
│  │  │ ─────────────────     │  │ ─────────────────     │              │    │
│  │  │ ระยะทาง: 400 km       │  │ ได้น้ำมัน: 13.94 L   │              │    │
│  │  │ ค่าใช้จ่าย: 717.60 B  │  │ ระยะทาง: 279 km      │              │    │
│  │  └───────────────────────┘  └───────────────────────┘              │    │
│  │                                                                     │    │
│  │  ┌─────────────────────────────────────────────────────────────┐   │    │
│  │  │ [Banknote] เติมเต็มถัง (36 ลิตร)                             │   │    │
│  │  │ ค่าใช้จ่าย: 1,292 บาท       ระยะทาง: 720 km                 │   │    │
│  │  └─────────────────────────────────────────────────────────────┘   │    │
│  │                                                                     │    │
│  │  * ระยะทางเป็นค่าประมาณการ อาจแตกต่างตามสภาพการขับขี่จริง        │    │
│  │                                                                     │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                             │
│  ┌─────────────── SECTION: COMPARISON BAR CHART ──────────────────────┐    │
│  │  เปรียบเทียบราคาน้ำมันทุกประเภท                                    │    │
│  │       ราคา (บาท/ลิตร)                                              │    │
│  │         │                                                           │    │
│  │   50.00 ┤                                                           │    │
│  │         │              ┌─────┐                                      │    │
│  │   43.44 ┤              │█████│                                      │    │
│  │   40.00 ┤    ┌─────┐   │█████│                                      │    │
│  │   35.88 ┤    │█████│   │█████│   ┌─────┐   ┌─────┐   ┌─────┐       │    │
│  │   30.00 ┤    │█████│   │█████│   │█████│   │█████│   │█████│       │    │
│  │    0.00 ┼────┴─────┴───┴─────┴───┴─────┴───┴─────┴───┴─────┴───    │    │
│  │           91       95      E20      B7      Hi B7                   │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                             │
│  ─────────────────────────────────────────────────────────────────────     │
│  Data from Bangchak                                                        │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 6. Component Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        app/page.tsx                             │
│                             │                                   │
│                             v                                   │
│              ┌──────────────────────────────┐                  │
│              │   OilPriceDashboard          │                  │
│              │   (Main Container)           │                  │
│              └──────────────────────────────┘                  │
│                             │                                   │
│     ┌───────────┬───────────┼───────────┬───────────┐          │
│     v           v           v           v           v          │
│ ┌────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌────────┐     │
│ │OilPrice│ │OilPrice │ │Distance │ │OilPrice │ │Remark  │     │
│ │Card    │ │History  │ │Calcul-  │ │Chart    │ │Footer  │     │
│ │(x8)    │ │Widget   │ │ator     │ │(Bar)    │ │        │     │
│ └────────┘ └─────────┘ └─────────┘ └─────────┘ └────────┘     │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                     API Route                                   │
│              /api/oil-prices/route.ts                          │
└─────────────────────────────────────────────────────────────────┘
```

---

## 7. Test Cases

### 7.1 Unit Tests - Oil Price Display

| Test ID | Component | Test Case | Expected Result |
|---------|-----------|-----------|-----------------|
| UT-001 | OilPriceCard | แสดงราคาถูกต้อง | ราคาแสดงในรูปแบบ XX.XX บาท/ลิตร |
| UT-002 | OilPriceCard | ราคาขึ้น (diff > 0) | Badge สีแดง + ไอคอน TrendingUp |
| UT-003 | OilPriceCard | ราคาลง (diff < 0) | Badge สีเขียว + ไอคอน TrendingDown |
| UT-004 | OilPriceCard | ราคาคงที่ (diff = 0) | Badge สีเทา + ไอคอน Minus |
| UT-005 | HistoryWidget | คำนวณ diff ถูกต้อง | diff = PriceToday - PriceYesterday |
| UT-006 | HistoryWidget | Tolerance check | diff < 0.001 = คงที่ |
| UT-007 | OilPriceChart | แสดง Bar Chart | แสดงแท่งกราฟตามจำนวน oil types |

### 7.2 Unit Tests - Distance Calculator (F-008)

| Test ID | Test Case | Input | Expected Result |
|---------|-----------|-------|-----------------|
| UT-008 | คำนวณระยะทางจากลิตร (Eco) | 20 L | 400 km (20 x 20) |
| UT-009 | คำนวณระยะทางจากลิตร (Sedan) | 20 L | 280 km (20 x 14) |
| UT-010 | คำนวณระยะทางจากลิตร (Pickup) | 20 L | 220 km (20 x 11) |
| UT-011 | คำนวณค่าใช้จ่าย | 20 L @ 35.88 | 717.60 บาท |
| UT-012 | คำนวณจากงบประมาณ | 500 B @ 35.88 | 13.94 L, 278.8 km |
| UT-013 | คำนวณเต็มถัง (Eco) | 36 L @ 35.88 | 1,291.68 B, 720 km |
| UT-014 | คำนวณเต็มถัง (Sedan) | 50 L @ 43.44 | 2,172 B, 700 km |
| UT-015 | คำนวณเต็มถัง (Pickup) | 76 L @ 32.44 | 2,465.44 B, 836 km |

### 7.3 Integration Tests

| Test ID | Test Case | Steps | Expected Result |
|---------|-----------|-------|-----------------|
| IT-001 | API Fetch Success | 1. Load page | แสดงข้อมูลราคาน้ำมันทั้งหมด |
| IT-002 | API Fetch Error | 1. Disconnect network<br>2. Load page | แสดง Error message + Retry button |
| IT-003 | Auto Refresh | 1. Load page<br>2. Wait 60s | ข้อมูลถูก refresh อัตโนมัติ |
| IT-004 | Manual Refresh | 1. Click Refresh button | ข้อมูลถูก refresh ทันที |
| IT-005 | Select Oil Type | 1. เลือก oil type ใน dropdown | กราฟแสดงข้อมูล oil ที่เลือก |
| IT-006 | Tab Switching | Click each car type tab | Specs update correctly |
| IT-007 | Input Fuel Amount | Enter 20 in fuel field | Show distance & cost |
| IT-008 | Input Budget | Enter 500 in budget field | Show liters & distance |
| IT-009 | Clear Input | Clear fuel field | Hide calculation results |

### 7.4 UI/UX Tests

| Test ID | Test Case | Expected Result |
|---------|-----------|-----------------|
| UX-001 | Loading State | แสดง Skeleton ขณะโหลดข้อมูล |
| UX-002 | Responsive Mobile | Cards แสดง 1 column บน mobile |
| UX-003 | Responsive Tablet | Cards แสดง 2 columns บน tablet |
| UX-004 | Responsive Desktop | Cards แสดง 3-5 columns บน desktop |
| UX-005 | Color Contrast | ทุก text อ่านได้ชัดเจน (WCAG AA) |

### 7.5 Edge Cases

| Test ID | Test Case | Input | Expected Result |
|---------|-----------|-------|-----------------|
| EC-001 | ราคาเท่ากันพอดี | Today=35.00, Yesterday=35.00 | แสดง "ราคาคงที่" (diff=0.00) |
| EC-002 | Floating point | Today=35.001, Yesterday=35.00 | แสดง "ราคาคงที่" (tolerance) |
| EC-003 | Null price | PriceToday=null | แสดง 0.00 หรือ N/A |
| EC-004 | Empty OilList | OilList=[] | แสดง Empty state |
| EC-005 | Missing icon | IconWeb=undefined | แสดง Fallback icon |
| EC-006 | Zero input | 0 L or 0 B | No calculation shown |
| EC-007 | Negative input | -10 L | Treat as 0 or prevent |
| EC-008 | Over tank size | Eco: 50 L | Allow but warn (optional) |
| EC-009 | Very large budget | 100,000 B | Calculate correctly |
| EC-010 | No oil data | oilList=[] | Show fallback or N/A |

---

## 8. Technical Stack

| Layer | Technology |
|-------|------------|
| Framework | Next.js 16 (App Router) |
| Language | TypeScript |
| Styling | Tailwind CSS v4 |
| UI Components | shadcn/ui |
| Charts | Recharts |
| Data Fetching | SWR |
| Icons | Lucide React |

---

## 9. File Structure

```
/vercel/share/v0-project/
├── app/
│   ├── api/
│   │   └── oil-prices/
│   │       └── route.ts          # API proxy
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx                   # Main page
├── components/
│   ├── ui/                        # shadcn components
│   ├── oil-price-card.tsx         # Price card component
│   ├── oil-price-chart.tsx        # Bar chart component
│   ├── oil-price-dashboard.tsx    # Main dashboard
│   ├── oil-price-history-widget.tsx # History comparison
│   └── distance-calculator.tsx    # Distance calculator
├── docs/
│   └── PRD-oil-price-dashboard.md # This document
└── package.json
```

---

## 10. Future Enhancements (Backlog)

| Feature | Description | Priority |
|---------|-------------|----------|
| F-009 | เก็บข้อมูลย้อนหลังใน Database | Low |
| F-010 | แจ้งเตือนเมื่อราคาเปลี่ยน (Push Notification) | Low |
| F-011 | เปรียบเทียบราคากับสถานีอื่น (PTT, Shell) | Low |
| F-012 | Export ข้อมูลเป็น CSV/PDF | Low |
| F-013 | Dark Mode Toggle | Low |
| F-014 | เพิ่มประเภทรถ (SUV, มอเตอร์ไซค์) | Low |
| F-015 | คำนวณค่าใช้จ่ายเดินทางระหว่างจุด | Low |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 27 Mar 2026 | Initial release with price display and charts |
| 1.1 | 27 Mar 2026 | Added Distance Calculator (F-008) |
