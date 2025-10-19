## 🗂 Общая структура проекта Laravel

```
app/
├── Console/
├── Exceptions/
├── Http/
│   ├── Controllers/
│   │   ├── Admin/
│   │   │   ├── DashboardController.php
│   │   │   ├── UserController.php
│   │   │   └── ...
│   │   ├── Manager/
│   │   │   ├── DashboardController.php
│   │   │   ├── TaskController.php
│   │   │   └── ...
│   │   ├── Venue/
│   │   │   ├── DashboardController.php
│   │   │   ├── EventController.php
│   │   │   └── ...
│   │   ├── Specialist/
│   │   │   ├── DashboardController.php
│   │   │   ├── BookingController.php
│   │   │   └── ...
│   │   ├── Client/
│   │   │   ├── DashboardController.php
│   │   │   ├── OrderController.php
│   │   │   └── ...
│   │   └── Controller.php
│   │
│   ├── Middleware/
│   │   ├── AdminMiddleware.php
│   │   ├── ManagerMiddleware.php
│   │   ├── VenueMiddleware.php
│   │   ├── SpecialistMiddleware.php
│   │   ├── ClientMiddleware.php
│   │   └── ...
│   │
│   ├── Requests/
│   │   ├── Admin/
│   │   ├── Manager/
│   │   ├── Venue/
│   │   ├── Specialist/
│   │   ├── Client/
│   │   └── ...
│   │
│   └── Kernel.php
│
├── Models/
│   ├── User.php
│   ├── Admin.php
│   ├── Manager.php
│   ├── Venue.php
│   ├── Specialist.php
│   ├── Client.php
│   └── ...
│
├── Policies/
├── Providers/
│   └── AuthServiceProvider.php
│
└── Services/
    ├── AdminService.php
    ├── ManagerService.php
    ├── VenueService.php
    ├── SpecialistService.php
    ├── ClientService.php
    └── ...
```

---

## 📁 resources/views

```
resources/
└── views/
    ├── layouts/
    │   ├── admin.blade.php
    │   ├── manager.blade.php
    │   ├── venue.blade.php
    │   ├── specialist.blade.php
    │   └── client.blade.php
    │
    ├── admin/
    │   ├── dashboard.blade.php
    │   ├── users.blade.php
    │   └── ...
    │
    ├── manager/
    │   ├── dashboard.blade.php
    │   ├── tasks.blade.php
    │   └── ...
    │
    ├── venue/
    │   ├── dashboard.blade.php
    │   ├── events.blade.php
    │   └── ...
    │
    ├── specialist/
    │   ├── dashboard.blade.php
    │   ├── bookings.blade.php
    │   └── ...
    │
    └── client/
        ├── dashboard.blade.php
        ├── orders.blade.php
        └── ...
```

---

## ⚙️ routes

```
routes/
├── web.php               # Общие маршруты
├── admin.php             # Маршруты для админов
├── manager.php           # Для менеджеров
├── venue.php             # Для площадок
├── specialist.php        # Для специалистов
└── client.php            # Для клиентов
```

Пример подключения в `RouteServiceProvider`:

```php
protected function mapAdminRoutes()
{
    Route::middleware(['web', 'auth', 'role:admin'])
        ->namespace($this->namespace . '\Admin')
        ->group(base_path('routes/admin.php'));
}
```

---



## 🧩 1. Blade Components (классический Laravel + Blade)

Laravel поддерживает Blade-компоненты из коробки.  
Все они хранятся в:

```
resources/views/components/
```

### 🔹 Рекомендуемая структура компонентов

```
resources/views/components/
├── layout/
│   ├── app.blade.php          # Основной layout
│   ├── admin.blade.php        # Layout для админки
│   ├── site.blade.php         # Layout для публичной части
│   └── modal.blade.php
│
├── ui/
│   ├── button.blade.php
│   ├── input.blade.php
│   ├── select.blade.php
│   ├── textarea.blade.php
│   ├── alert.blade.php
│   └── card.blade.php
│
├── navigation/
│   ├── navbar.blade.php
│   ├── sidebar.blade.php
│   ├── breadcrumbs.blade.php
│   └── footer.blade.php
│
├── forms/
│   ├── login-form.blade.php
│   ├── register-form.blade.php
│   └── contact-form.blade.php
│
└── tables/
    ├── data-table.blade.php
    ├── row.blade.php
    └── cell.blade.php
```

---

### 🔹 Пример Blade-компонента

`resources/views/components/ui/button.blade.php`

```blade
@props(['type' => 'button', 'color' => 'primary'])

<button type="{{ $type }}" {{ $attributes->merge(['class' => "px-4 py-2 rounded bg-{$color}-500 text-white hover:bg-{$color}-600"]) }}>
    {{ $slot }}
</button>
```

Использование:

```blade
<x-ui.button color="blue">Сохранить</x-ui.button>
```

---

### 🔹 Компоненты по ролям (если нужно разделение)

```
resources/views/components/
├── admin/
│   ├── sidebar.blade.php
│   └── stats-card.blade.php
├── manager/
│   └── task-card.blade.php
├── client/
│   └── order-summary.blade.php
└── site/
    ├── hero-section.blade.php
    ├── feature-card.blade.php
    └── testimonials.blade.php
```

---
