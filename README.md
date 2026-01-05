# Bookstore-E-Commerce-Web-Application

# 📚 Cyber Bookstore – E-Commerce Web Application


---

## 🎯 Maqsad
Kitob do‘koni uchun kichik e-commerce tizim yaratish:

* REST API dizayn
* Authentication & Authorization (JWT, Permission)
* Business Logic & Validation
* Performance (ORM optimizatsiya)
* API hujjatlashtirish

Foydalanuvchi onlayn kitob tanlaydi, savatchaga qo‘shadi, ro‘yxatdan o‘tadi va xarid qiladi. Admin esa kitoblarni boshqaradi, sotuvlarni ko‘radi va ID kodlarni tekshiradi.

---

## 1️⃣ Texnologiyalar (majburiy)
* Python 3.x
* Django
* Django Rest Framework (DRF)
* JWT Authentication (`djangorestframework-simplejwt`)
* PostgreSQL
* Swagger / Redoc (`drf-spectacular`)
* `.env` (environment variables)
* Git + GitHub (public repository)

---

## 2️⃣ Foydalanuvchi rollari

| Role        | Tavsif                                                       |
| ----------- | ------------------------------------------------------------ |
| **Admin**   | Tizimdagi barcha kitoblar va xaridlarni boshqaradi           |
| **User**    | Kitoblarni ko‘rib chiqadi, savatchaga qo‘shadi va xarid qiladi, profilni boshqaradi, wishlist va sharhlar qo‘shadi |

---

## 3️⃣ Ma’lumotlar modellari

### 📖 Book

```text
id
title
author
price
stock_count
description
created_at
updated_at
```

## 🛒 CartItem
```
id
user (FK → User)
book (FK → Book)
quantity
added_at
```

## 🧾 Order
```
id
user (FK → User)
books (ManyToMany → Book through CartItem)
total_price
status (pending / completed / cancelled)
payment_method (online / in_store)
pickup_code (6-8 raqamli ID)  # Agar offline xarid bo‘lsa
created_at
updated_at
```

## 👤 UserProfile
```
id
user (OneToOne → User)
full_name
address
phone
date_of_birth
```

## 💖 Wishlist
```
id
user (FK → User)
book (FK → Book)
added_at
```

## ⭐ Rating & Review
```
id
user (FK → User)
book (FK → Book)
rating (1-5)
review_text
created_at
updated_at
```


## 4️⃣ Funksional talablar

## 🔐 Authentication & Authorization
```
User register/login

JWT access & refresh token

Role-based permission (Admin / User)
```
## 👤 User imkoniyatlari
```

Kitoblarni ko‘rib chiqish

Kitobni savatchaga qo‘shish / o‘chirish

Savatcha orqali xarid qilish:

Online to‘lov (agar integratsiya mavjud bo‘lsa)

Offline – 6–8 raqamli pickup ID olish

O‘z buyurtmalarini ko‘rish

Profilni boshqarish (UserProfile)

Wishlistga kitob qo‘shish / o‘chirish

Kitoblarga reyting va sharh qo‘yish
```

## 🛡 Admin imkoniyatlari
```

Kitob CRUD (Create, Read, Update, Delete)

Kitob sonini oshirish yoki kamaytirish

Sotilgan kitoblar ro‘yxatini ko‘rish

Qolmagan kitoblar ro‘yxatini ko‘rish

Foydalanuvchi buyurtmalarini ko‘rish va ID kodlarni tekshirish

Foydalanuvchi profillari va sharhlarni boshqarish
```

## 5️⃣ Business Logic / Validation
```

❌ Kitob soni 0 dan kam bo‘lmasligi kerak

❌ Xarid qilishda kitob stock_count dan oshmasligi kerak

✅ Offline xarid bo‘lsa → pickup_code yaratiladi va yagona bo‘ladi

✅ Order cancelled bo‘lsa → kitob stock_count tiklanadi

❌ Foydalanuvchi boshqa user buyurtmalarini ko‘ra olmaydi

✅ Wishlistdagi kitoblarni faqat o‘zi ko‘ra oladi

✅ Reyting 1-5 oralig‘ida bo‘lish
```


## 6️⃣ API Endpointlar (minimum requirement)

**Authentication**


| Method | Endpoint               | Description                   | Access |
| ------ | ---------------------- | ----------------------------- | ------ |
| POST   | `/auth/register/`      | Yangi user ro‘yxatdan o‘tishi | Public |
| POST   | `/auth/login/`         | Login va JWT token olish      | Public |
| POST   | `/auth/token/refresh/` | Tokenni yangilash             | Auth   |

**Books**

| Method | Endpoint       | Description              | Access |
| ------ | -------------- | ------------------------ | ------ |
| GET    | `/books/`      | Barcha kitoblar ro‘yxati | Public |
| GET    | `/books/{id}/` | Kitob detail             | Public |
| POST   | `/books/`      | Yangi kitob qo‘shish     | Admin  |
| PATCH  | `/books/{id}/` | Kitobni tahrirlash       | Admin  |
| DELETE | `/books/{id}/` | Kitobni o‘chirish        | Admin  |


**Cart**

| Method | Endpoint      | Description                   | Access |
| ------ | ------------- | ----------------------------- | ------ |
| GET    | `/cart/`      | Foydalanuvchi savatchasi      | User   |
| POST   | `/cart/`      | Kitob qo‘shish                | User   |
| PATCH  | `/cart/{id}/` | Kitob miqdorini o‘zgartirish  | User   |
| DELETE | `/cart/{id}/` | Kitobni savatchadan o‘chirish | User   |


**Orders**

| Method | Endpoint        | Description                 | Access      |
| ------ | --------------- | --------------------------- | ----------- |
| POST   | `/orders/`      | Buyurtma yaratish           | User        |
| GET    | `/orders/me/`   | Foydalanuvchi buyurtmalari  | User        |
| GET    | `/orders/`      | Barcha buyurtmalar ro‘yxati | Admin       |
| GET    | `/orders/{id}/` | Buyurtma detail             | Owner/Admin |
| PATCH  | `/orders/{id}/` | Statusni o‘zgartirish       | Admin       |
| DELETE | `/orders/{id}/` | Buyurtmani bekor qilish     | User/Admin  |

**UserProfile**

| Method | Endpoint        | Description                 | Access      |
| ------ | --------------- | --------------------------- | ----------- |
| POST   | `/orders/`      | Buyurtma yaratish           | User        |
| GET    | `/orders/me/`   | Foydalanuvchi buyurtmalari  | User        |
| GET    | `/orders/`      | Barcha buyurtmalar ro‘yxati | Admin       |
| GET    | `/orders/{id}/` | Buyurtma detail             | Owner/Admin |
| PATCH  | `/orders/{id}/` | Statusni o‘zgartirish       | Admin       |
| DELETE | `/orders/{id}/` | Buyurtmani bekor qilish     | User/Admin  |


**Wishlist**

| Method | Endpoint          | Description             | Access |
| ------ | ----------------- | ----------------------- | ------ |
| GET    | `/wishlist/`      | O‘z wishlistini ko‘rish | User   |
| POST   | `/wishlist/`      | Kitob qo‘shish          | User   |
| DELETE | `/wishlist/{id}/` | Kitobni o‘chirish       | User   |


**Rating & Review**


| Method | Endpoint               | Description               | Access      |
| ------ | ---------------------- | ------------------------- | ----------- |
| GET    | `/books/{id}/reviews/` | Kitob sharhlari ro‘yxati  | Public      |
| POST   | `/books/{id}/reviews/` | Reyting va sharh qo‘shish | User        |
| PATCH  | `/reviews/{id}/`       | Sharhni yangilash         | Owner       |
| DELETE | `/reviews/{id}/`       | Sharhni o‘chirish         | Owner/Admin |

## 8️⃣ Loyihaning struktura

```
cyber-bookstore/
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

## 9️⃣ Role–Endpoint Matrix

| Endpoint              | Admin   | User    |
| --------------------- | ------- | ------- |
| Auth (login/register) | ✅       | ✅       |
| Books CRUD            | ✅       | ❌       |
| View Books            | ✅       | ✅       |
| Cart CRUD             | ❌       | ✅       |
| Create Order          | ❌       | ✅       |
| View Own Orders       | ✅       | ✅       |
| View All Orders       | ✅       | ❌       |
| Change Order Status   | ✅       | ❌       |
| Cancel Order          | ✅       | ✅       |
| Profile CRUD          | ❌       | ✅       |
| Wishlist CRUD         | ❌       | ✅       |
| Rating & Review CRUD  | ✅/Owner | ✅/Owner |

## 📝 Swagger & API Docs

Barcha endpointlar Swagger yoki Redoc orqali hujjatlashtirilgan.

Request/Response misollar keltirilgan.

Query param va serializer validation mavjud.