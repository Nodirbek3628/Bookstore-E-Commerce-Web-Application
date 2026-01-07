## 📚 **Bookstore E-Commerce API (Kitob do‘koni)**

* REST API dizayn
* Authentication & Authorization (JWT, Permission)
* Business Logic & Validation
* Performance (ORM optimizatsiya)
* API hujjatlashtirish

---

## 1️⃣ TEXNOLOGIYALAR (MAJBURIY)

Talaba quyidagi texnologiyalarni **to‘liq va to‘g‘ri** ishlatishi shart:

* Python 3.12
* Django
* Django Rest Framework (DRF)
* JWT Authentication (`djangorestframework-simplejwt`)
* PostgreSQL
* Swagger / Redoc (`drf-spectacular`)
* `.env` (environment variables)
* Git + GitHub (public repository)

---

## 2️⃣ FOYDALANUVCHI ROLLARI

| Role        | Tavsif                                             |
| ----------- | -------------------------------------------------- |
| **Admin**   | Kitoblar va buyurtmalarni to‘liq boshqaradi        |
| **User**    | Kitoblarni ko‘radi va sotib oladi                  |

---

## 3️⃣ MA’LUMOTLAR MODELLARI

### 👤 User (Custom User)

```text
id
phone_number (unique)
password
is_active
is_staff
created_at
```
### 📘 Book

```text
id
title
author
price
quantity
description
created_at
```
### 🛒 Cart
```text
id
user (OneToOne → User)
created_at
```
### 🧾 CartItem

```
id
cart (ForeignKey → Cart)
book (ForeignKey → Book)
quantity

```
### 📦 Order

```
id
user (ForeignKey → User)
total_price
order_code (6–8 xonali ID)
status (pending / completed / cancelled)
created_at

```

### 📚 OrderItem

```
id
order (ForeignKey → Order)
book (ForeignKey → Book)
quantity
price

```
## 4️⃣ FUNKSIONAL TALABLAR

### 🔐 Authentication & Authorization

- Telefon raqam orqali register
- Login
- JWT access & refresh token
- Role-based permission

---

### 👤 User imkoniyatlari

- Kitoblar ro‘yxatini ko‘rish
- Kitobni cart’ga qo‘shish
- Cart’dan kitobni o‘chirish
- Buyurtma berish (checkout)
- O‘z buyurtmalarini ko‘rish

❌ Boshqa user buyurtmalarini ko‘ra olmaydi

---

### 🛡 Admin imkoniyatlari

- Kitob CRUD (create, update, delete)
- Kitob sonini boshqarish
- Barcha buyurtmalarni ko‘rish
- Offline buyurtmalarni ID orqali tekshirish
- Sotilib tugagan kitoblarni ko‘rish

---

## 5️⃣ BUSINESS LOGIC (ASOSIY BAHOLANADIGAN QISM)

Quyidagi **validation va mantiqiy shartlar** majburiy:

### ✅ Validation Rules

- ❌ Kitob quantity 0 dan kichik bo‘lishi mumkin emas
- ❌ Cart’ga mavjud bo‘lmagan kitob qo‘shilmaydi
- ❌ Cart’dagi quantity `book.quantity` dan katta bo‘lishi mumkin emas
- ✅ Order yaratilganda:
  - kitob quantity kamayadi
  - cart tozalanadi
- ❌ Quantity 0 bo‘lgan kitob sotib olinmaydi
- ❌ User o‘zidan boshqa buyurtmani o‘zgartira olmaydi

---

## 6️⃣ PERMISSION TALABLARI

Quyidagi custom permission’lar bo‘lishi shart:

- `IsAdmin`
- `IsAuthenticatedUser`
- `IsOwner`

📌 Misollar:

- User → faqat **o‘z cart va orderlari**
- Admin → **barcha ma’lumotlar**

---

## 7️⃣ API ENDPOINTLAR (MINIMUM REQUIREMENT)

```http
POST   /auth/register/
POST   /auth/login/
POST   /auth/token/refresh/

GET    /books/
GET    /books/{id}/

POST   /cart/items/
GET    /cart/
DELETE /cart/items/{id}/

POST   /orders/
GET    /orders/me/
```

## 8️⃣ QO‘SHIMCHA TALABLAR (PLUS BALL)

- Pagination
- Filtering:
  - price
  - author
- Search:
  - book title
  - author
- Serializer level validation
- `select_related` / `prefetch_related` ishlatilgan bo‘lishi shart
- Clean architecture (views, serializers, permissions ajratilgan)

---

## 9️⃣ SWAGGER & README (MAJBURIY)

### Swagger

- Barcha endpointlar hujjatlashtirilgan
- Request / Response example’lar mavjud

---

### README ichida bo‘lishi shart

- Project setup (local ishga tushirish)
- `.env.example`
- Migration & superuser yaratish
- API’dan foydalanish tartibi
- Role’lar va ularning imkoniyatlari

---

## 📁 PROJECT STRUCTURE

```text
bookstore_api/
├── apps/
│   ├── users/
│   │   ├── models.py
│   │   ├── serializers.py
│   │   ├── views.py
│   │   ├── permissions.py
│   ├── books/
│   │   ├── models.py
│   │   ├── serializers.py
│   │   ├── views.py
│   ├── cart/
│   │   ├── models.py
│   │   ├── serializers.py
│   │   ├── views.py
│   ├── orders/
│   │   ├── models.py
│   │   ├── serializers.py
│   │   ├── views.py
├── core/
│   ├── settings.py
│   ├── urls.py
├── .env.example
├── requirements.txt
├── README.md
```

## 📌 ALL ENDPOINTS (FULL LIST)

### Base URL

```text
/api
```

### Authentication
```
Authorization: Bearer <access_token>
```

## 🔐 AUTHENTICATION & USER

| Method | Endpoint               | Description                      | Access |
| ------ | ---------------------- | -------------------------------- | ------ |
| POST   | `/auth/register/`      | Telefon orqali ro‘yxatdan o‘tish | Public |
| POST   | `/auth/login/`         | Login va JWT token olish         | Public |
| POST   | `/auth/token/refresh/` | Access tokenni yangilash         | Auth   |
| GET    | `/auth/me/`            | Joriy user ma’lumotlari          | Auth   |

---

## 📘 BOOKS

| Method | Endpoint         | Description           | Access |
| ------ | ---------------- | --------------------- | ------ |
| GET    | `/books/`        | Kitoblar ro‘yxati     | Public |
| GET    | `/books/{id}/`   | Kitob detail          | Public |
| POST   | `/books/`        | Kitob qo‘shish        | Admin  |
| PATCH  | `/books/{id}/`   | Kitobni yangilash     | Admin  |
| DELETE | `/books/{id}/`   | Kitobni o‘chirish     | Admin  |

---

## 🛒 CART

| Method | Endpoint            | Description        | Access |
| ------ | ------------------- | ------------------ | ------ |
| GET    | `/cart/`            | Cart ko‘rish       | User   |
| POST   | `/cart/items/`      | Kitob qo‘shish     | User   |
| DELETE | `/cart/items/{id}/` | Cart’dan o‘chirish | User   |

---

## 📦 ORDERS

| Method | Endpoint      | Description          | Access |
| ------ | ------------- | -------------------- | ------ |
| POST   | `/orders/`    | Buyurtma yaratish    | User   |
| GET    | `/orders/me/` | Mening buyurtmalarim | User   |
| GET    | `/orders/`    | Barcha buyurtmalar   | Admin  |

---

## 🛡 ROLE–ENDPOINT MATRIX (QISQA)

| Endpoint        | Admin | User |
| --------------- | ----- | ---- |
| Auth            | ✅     | ✅    |
| Books CRUD      | ✅     | ❌    |
| View Books      | ✅     | ✅    |
| Cart            | ❌     | ✅    |
| Create Order    | ❌     | ✅    |
| View All Orders | ✅     | ❌    |

