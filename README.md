# 🛒 Electronics Store Management System (ESMS)

نظام متكامل لإدارة متجر إلكترونيات — Backend مبني بـ **.NET Web API** وFrontend مبني بـ **React**، يديروا الكاتيجوريز والموردين والمنتجات والعملاء والأوردرات بعمليات CRUD كاملة.

---

## 🔗 الروابط المباشرة (Live)

| | الرابط |
|---|---|
| 🚀 الموقع (Frontend) | https://esms-frontend-snowy.vercel.app |
| 📦 Backend Repo (GitHub) | https://github.com/samamokhtar8m-max/Electronics-Store-Management-System-ESMS-.git |
| 📦 Frontend Repo (GitHub) | https://github.com/samamokhtar8m-max/esms_Full-Front-Back-.git |

---

## 🛠️ Tech Stack

### Backend
- **.NET Web API** (.NET 8)
- **Entity Framework Core** + SQL Server
- **Repository Pattern** + **Specification Pattern** (فلترة وIncludes قابلة لإعادة الاستخدام)
- **AutoMapper** لتحويل الـ Entities إلى DTOs
- **Dependency Injection** لكل الـ Repositories والـ Services
- **Swagger / Swashbuckle** لتوثيق واختبار الـ Endpoints
- مستضاف على **Somee.com** (Free ASP.NET Hosting + MS SQL Express)

### Frontend
- **React** (Vite)
- **React Router** للتنقل بين الصفحات
- **Axios** للتواصل مع الـ API
- مستضاف على **Vercel**

---

## ✨ المميزات

- CRUD كامل لكل من: **Categories, Suppliers, Products, Customers, Orders**
- عرض تفصيلي: الموردين مع منتجاتهم، والعملاء مع أوردراتهم
- إنشاء أوردر باختيار عميل موجود + منتجات متعددة بكمياتها، مع **حساب الإجمالي تلقائيًا من السيرفر** بناءً على السعر الحالي لكل منتج (مش من الكلاينت)
- التحقق من صحة الـ Foreign Keys (Category/Supplier/Product/Customer) قبل الحفظ في كل العمليات
- تحديث تلقائي للجداول بعد كل عملية إضافة/تعديل/حذف

---

## 🏗️ معمارية الـ Backend

```
ElectronicsStoreManagementSystem
│
├── Controllers          # 5 controllers (Category, Supplier, Product, Customer, Order)
├── Models                # الكيانات (Entities) والعلاقات بينها
├── Dto                   # DTOs للإدخال والإخراج، منفصلة عن الـ Models
├── GenericRepository      # IGenericRepository / GenericRepository (CRUD عام لكل الكيانات)
├── Specifications         # Ispecifications / Basespecifications / SpecifactionEvlutor
│                          # + Spec خاص لكل Entity (فلترة و Includes)
├── Helper
│   └── MappingProfile.cs  # كل الـ mappings بين Models والـ DTOs (AutoMapper)
├── Data
│   └── ApplicationDbContext.cs
└── Program.cs             # تسجيل الـ DI, DbContext, AutoMapper, CORS
```

### العلاقات بين الكيانات
- **Category ↔ Product**: One-to-Many
- **Supplier ↔ Product**: One-to-Many
- **Customer ↔ Order**: One-to-Many
- **Order ↔ OrderItem**: One-to-Many
- **Product ↔ OrderItem**: One-to-Many
- **Order ↔ Product**: Many-to-Many (عن طريق جدول `OrderItem`)

---

## 🏗️ معمارية الـ Frontend

```
src/
├── api/
│   └── axiosClient.js     # نقطة اتصال واحدة مركزية بالـ API (baseURL)
├── services/               # نداءات الـ API لكل entity (categoryService, orderService...)
├── components/             # الفورمات والجداول لكل entity
├── pages/                  # صفحة كاملة لكل entity (CRUD)
└── App.jsx                 # الـ Routing (react-router-dom)
```

---

## 🚀 التشغيل محليًا

### Backend
```bash
# عدّل appsettings.json بالـ connection string بتاعك
dotnet restore
dotnet ef database update
dotnet run
```
هيفتح Swagger على: `https://localhost:<port>/swagger`

### Frontend
```bash
cd esms-frontend
npm install
npm run dev
```
افتح `http://localhost:5173`

⚠️ لازم تتأكد إن رابط الـ API في `src/api/axiosClient.js` مطابق للبيئة اللي بتشتغل عليها (محلي أو الرابط اللايف).

---

## 📋 الـ Endpoints الرئيسية

| Method | Endpoint | الوصف |
|---|---|---|
| GET/POST | `/api/Category` | كل الكاتيجوريز / إضافة |
| GET/PUT/DELETE | `/api/Category/{id}` | تفاصيل / تعديل / حذف |
| GET/POST | `/api/Supplier` | كل الموردين / إضافة |
| GET | `/api/Supplier/{id}` | تفاصيل المورد + منتجاته |
| GET/POST | `/api/Product` | كل المنتجات (مع الكاتيجوري والمورد) / إضافة |
| GET/POST | `/api/Customer` | كل العملاء / إضافة |
| GET | `/api/Customer/{id}` | تفاصيل العميل + أوردراته |
| GET/POST | `/api/Order` | كل الأوردرات / إنشاء أوردر (حساب تلقائي للإجمالي) |
| PUT | `/api/Order/{id}` | تعديل منتجات/كميات الأوردر وإعادة حساب الإجمالي |

---

## 🔭 توسعات مستقبلية ممكنة

- صفحة تسجيل دخول وصلاحيات مستخدمين
- بحث وفلترة متقدمة في الجداول
- تصدير التقارير PDF/Excel
- Pagination للجداول الكبيرة
