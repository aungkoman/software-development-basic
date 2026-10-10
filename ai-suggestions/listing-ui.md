ဒီအချက်တွေကို Software House တစ်ခုရဲ့ Standard CRUD Module Development Guidelines အဖြစ် သတ်မှတ်ထားလို့ရပါတယ်။

Listing, Create, Edit, Detail Module တွေကို Project တိုင်းမှာ တစ်ပုံစံတည်း၊ ပြည့်ပြည့်စုံစုံ Implement လုပ်နိုင်ဖို့ အောက်ပါအတိုင်း Standard သတ်မှတ်ဖို့ အကြံပြုပါတယ်။

# Standard CRUD Module Requirements (v1.0)

## 1. Listing Module

| Feature            | Requirement                                            |
| ------------------ | ------------------------------------------------------ |
| Searching          | Keyword Search, Debounce                               |
| Filtering          | Dropdown, Foreign Key, Date Range, Status              |
| Sorting            | Column-wise ASC / DESC                                 |
| Pagination         | Server-side Pagination, Page Size                      |
| Summary Cards      | Total Items, Filtered Items, Current Page, Total Pages |
| Row Actions        | Detail, Edit, Delete                                   |
| Column Visibility  | Show / Hide Columns                                    |
| Column Preference  | Local Storage မှာ သိမ်းထားရန်                          |
| Multiple Selection | Checkbox, Select All                                   |
| Bulk Actions       | Bulk Delete, Status Update, Export                     |
| Loading            | Skeleton / Shimmer                                     |
| Caching            | Query Cache / State Management                         |
| Empty State        | No Data / No Search Results                            |
| Export             | CSV / Excel (လိုအပ်သလို)                               |

Filter Logic

Filter အားလုံးကို default အနေဖြင့် `AND` condition နဲ့ တွဲမယ်။

ဥပမာ -

```
WHERE status = 'ACTIVE'
  AND region_id = 1
  AND township_id = 5
  AND name LIKE '%aung%'
```

Active Filter တွေကို Listing အပေါ်မှာ Chip ပုံစံနဲ့ ပြမယ်။ Filter တစ်ခုချင်းစီ ဖယ်ရှားနိုင်ရမယ်။ `Clear All` ပါရမယ်။

## 2. Listing UI နမူနာ

Customer Management

&#x20;New

Total Items

# 4

Filtered Items

# 4

Current Page

# 1

Total Pages

# 2

All Status

All Regions

Clear All

&#x20;Columns

|   | NAME      | REGION   | STATUS | ACTION |
| - | --------- | -------- | ------ | ------ |
|   | Aung Aung | Yangon   | Active |        |
|   | Su Su     | Mandalay | Active |        |

Page 1 of 2

Interactive UI နမူနာဖြစ်ပြီး demo data ကိုသာ အသုံးပြုထားသည်။ Backend နှင့် local persistence မချိတ်ထားသေးပါ။

## 3. Create / Edit Form Standard

- Client-side နဲ့ Server-side Validation နှစ်မျိုးလုံး
- Field တစ်ခုချင်းစီအလိုက် Error Message
- Submit Button အပေါ်မှာ Validation Error Summary
- Dropdown Loading Indicator
- Region → Township Cascade Dropdown
- Long Form များအတွက် Stepper / Accordion
- Create / Edit ကို Modal, Drawer (Side Panel), Full Page အနေနဲ့ အသုံးပြုနိုင်ရန်
- Unsaved Changes Warning
- Submit လုပ်နေစဉ် Double Submit မဖြစ်အောင် Button Disable
- Success / Error Notification
- Server ကပြန်လာတဲ့ Validation Error ကို သက်ဆိုင်ရာ Field မှာပြရန်

## 4. State Management & Caching

Next.js + MUI Project တွေအတွက် TanStack Query + Zustand ကို အကြံပြုပါတယ်။

TanStack Query

- API data caching
- Query key အလိုက် Filter / Sort / Search / Pagination state ခွဲထားခြင်း
- Background Refetch
- Create / Update / Delete ပြီးနောက် သက်ဆိုင်ရာ Cache Invalidation
- Page ပြန်ရောက်လာတဲ့အခါ Cached Data ချက်ချင်းပြခြင်း

Zustand / Local Storage

- Column Visibility
- Page Size Preference
- Filter Preference
- Table Density
- View Settings

Cache ကို အမြဲသိမ်းထားတာထက် `staleTime`, `gcTime`, persistence policy သတ်မှတ်သင့်ပါတယ်။ Sensitive Data တွေကို Local Storage မှာ မသိမ်းသင့်ပါ။

## 5. Backend API Standard

```
GET /api/v1/customers
  ?page=1
  &pageSize=20
  &search=aung
  &sortBy=createdAt
  &sortOrder=desc
  &status=ACTIVE
  &regionId=1
  &townshipId=5
```

Response Format:

```
{
  "success": true,
  "data": [],
  "meta": {
    "total": 250,
    "filteredTotal": 45,
    "page": 1,
    "pageSize": 20,
    "totalPages": 3
  }
}
```

API က Server-side Filtering, Sorting, Searching, Pagination, Validation, Authorization နဲ့ Bulk Operations တွေကို support လုပ်ရပါမယ်။

အထူးသဖြင့် `sortBy` နဲ့ Filter Field တွေကို whitelist စစ်ဆေးပြီး SQL Injection မဖြစ်အောင် parameterized query အသုံးပြုရပါမယ်။

## 6. ထပ်ဖြည့်သင့်တဲ့ Features

1. URL Query Sync — Filter, Search, Sort, Page တွေကို URL နဲ့ sync လုပ်ပြီး link share လုပ်နိုင်ရန်။
2. Saved Views — အသုံးများတဲ့ Filter Configuration တွေကို သိမ်းထားနိုင်ရန်။
3. Permission-based Actions — User Role အလိုက် Create, Edit, Delete, Export ခွင့်ပြုရန်။
4. Audit Trail — ဘယ်သူက ဘယ်အချိန် ဘာပြင်ခဲ့သလဲ သိနိုင်ရန်။
5. Optimistic Updates — သင့်တော်တဲ့ Action တွေမှာ UI ကို ချက်ချင်း Update လုပ်ပြီး Failure ဖြစ်ရင် Rollback လုပ်ရန်။
6. Bulk Action Confirmation — Delete လိုမျိုး အရေးကြီးတဲ့ Action တွေအတွက် Confirmation Dialog။
7. Accessibility — Keyboard Navigation, Screen Reader Label, Focus Management။
8. Responsive Design — Mobile မှာ Table ကို Card View သို့မဟုတ် Horizontal Scroll နဲ့ အသုံးပြုနိုင်ရန်။

ဒီ Standard ကို နောက်ပိုင်း Laravel, Next.js, React Project တွေမှာ Reusable CRUD Module Template အဖြစ် တည်ဆောက်ထားမယ်ဆိုရင် Development Time လျှော့ချနိုင်ပြီး UI/UX နဲ့ API Design တွေ တစ်သမတ်တည်းဖြစ်လာပါမယ်။