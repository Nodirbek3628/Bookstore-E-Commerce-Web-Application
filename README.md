# 📚 FINAL EXAM PROJECT

## **Book Store – E-Commerce Backend API**

### 🎯 Maqsad

Kitob do‘koni uchun mo‘ljallangan **kichik e-commerce tizim** yaratish orqali talabaning:

- REST API dizayn
- Authentication & Authorization
- Business logic
- Admin & User flow
- Real savdo jarayoni mantiqi

bo‘yicha ko‘nikmalarini baholash.

---

## 1️⃣ TEXNOLOGIYALAR (MAJBURIY)

- Python 3.x  
- Django  
- Django Rest Framework (DRF)  
- JWT Authentication (`djangorestframework-simplejwt`)  
- PostgreSQL  
- Swagger / Redoc (`drf-spectacular`)  
- `.env` (environment variables)  
- Git + GitHub  

---

## 2️⃣ FOYDALANUVCHI TOMONLARI

Loyiha **2 asosiy qismdan** iborat:

- **User (Foydalanuvchi)**
- **Admin (Boshqaruv paneli)**

---

## 3️⃣ USER TOMONI (FOYDALANUVCHI)

### 📖 Imkoniyatlar

- Kitoblar ro‘yxatini ko‘rish  
- Kitob tafsilotlarini ko‘rish  
- Savatchaga (cart) kitob qo‘shish  
- Xarid qilish  

---

### 🔐 Ro‘yxatdan o‘tish / Login

- Telefon raqam orqali ro‘yxatdan o‘tish  
- Telefon raqam orqali login qilish  
- JWT token asosida autentifikatsiya  

---

### 💳 Xarid jarayoni

Xarid tugagandan so‘ng **2 xil holat** mavjud:

1️⃣ **Online to‘lov mavjud bo‘lsa**
- Foydalanuvchi to‘lovni platforma orqali amalga oshiradi
- Buyurtma `paid` holatiga o‘tadi

2️⃣ **Online to‘lov mavjud bo‘lmasa**
- Foydalanuvchiga **6–8 xonali ID kod** beriladi
- Foydalanuvchi shu ID bilan do‘kondan kitobni olib ketadi

---

## 4️⃣ ADMIN PANEL

Admin panel quyidagi bo‘limlardan iborat:

---

### 📚 Kitoblarni boshqarish

- Yangi kitob qo‘shish:
  - nomi
  - muallifi
  - narxi
  - soni
  - tavsifi
- Kitobni:
  - tahrirlash (edit)
  - o‘chirish (delete)
  - sonini oshirish / kamaytirish

---

### 🛒 Sotib olingan kitoblar

- Xarid qilingan barcha kitoblar ro‘yxati
- Agar offline xarid bo‘lsa — **ID kodi** ko‘rinadi
- Buyurtma holati (pending / paid / completed)

---

### ❌ Qolmagan kitoblar

- Sotilib tugagan kitoblar alohida ro‘yxatda chiqadi
- Stock = 0 bo‘lgan mahsulotlar

---

## 5️⃣ MA’LUMOTLAR MODELLARI

### 👤 User

```text
id
phone_number
is_active
created_at
```
## 5️⃣ MA’LUMOTLAR MODELLARI

### 📘 Book

```text
id
title
author
price
stock
description
created_at
```
## 🛒 Cart

```text
id
user (FK)
updated_at
```
### 🛍 CartItem
```
id
cart (FK)
book (FK)
quantity

```

### 📦 Order
```
id
user (FK)
total_price
order_code (6–8 digit ID)
status (pending / paid / completed)
created_at

```
### ## 6️⃣ BUSINESS LOGIC (ASOSIY BAHOLANADI)

### ✅ Validation Rules

- ❌ Stock `0` bo‘lsa kitob sotib olinmaydi  
- ❌ Cart’da bir kitob takror qo‘shilmaydi  
- ✅ Xarid bo‘lganda kitob soni kamayadi  
- ❌ Begona user buyurtmasiga kira olmaydi  
- ✅ Offline xaridda **ID kod avtomatik yaratiladi**  

---

## 7️⃣ PERMISSION TALABLARI

Custom permission’lar:

- `IsAdmin`
- `IsAuthenticated`
- `IsOwner`

📌 Misollar:

- User → faqat **o‘z cart va orderlari**
- Admin → **barcha ma’lumotlar**

---

## 8️⃣ API ENDPOINTLAR (MINIMUM)

```http
POST   /auth/login/
POST   /auth/register/

GET    /books/
GET    /books/{id}/

POST   /cart/items/
GET    /cart/

POST   /orders/
GET    /orders/me/

GET    /admin/orders/
```
## 9️⃣ SWAGGER & README (MAJBURIY)

### Swagger

- Barcha endpointlar hujjatlashtirilgan  
- Request / Response misollar mavjud  

### README ichida

- Project setup  
- `.env.example`  
- Migration & superuser  
- API’dan foydalanish  

---

## 📁 PROJECT STRUCTURE

```
book_store_api/
├── apps/
│   ├── users/
│   ├── books/
│   ├── cart/
│   ├── orders/
│   ├── payments/
├── core/
│   ├── settings.py
│   ├── urls.py
├── .env.example
├── requirements.txt
├── README.md
```


---

## 👨‍💻 Author

**Nodirbek Abloqulov**  
Backend Developer (Python / Django / DRF)
