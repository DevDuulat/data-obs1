## **S — Single Responsibility (Единственная ответственность)**

Каждый класс решает **только одну задачу**.

```php
class User {
    public function save() { /* сохраняет пользователя */ }
}

class UserMailer {
    public function sendWelcomeEmail(User $user) { /* отправка email */ }
}
```

- `User` — данные пользователя
- `UserMailer` — только почта

**Зачем:** изменения в одном классе не ломают другой.

---

## **O — Open/Closed (Открытость/Закрытость)**

Классы **открыты для расширения, закрыты для изменения**.

```php
interface PaymentMethod { function pay($amount); }

class CreditCard implements PaymentMethod { function pay($amount) {} }
class PayPal implements PaymentMethod { function pay($amount) {} }
```

- Новый способ оплаты → добавляем новый класс, **не меняя старый**.
    

---

## **L — Liskov Substitution (Подстановка Лисков)**

Класс-потомок должен **заменять класс-родитель без ошибок**.

```php
class Bird { function fly() {} }
class Sparrow extends Bird { function fly() {} } // ок
class Penguin extends Bird { function fly() { throw new Exception(); } } // нарушение
```

- Пингвин не умеет летать → нарушает принцип.
- Решение: разделить интерфейсы на `FlyingBird` и `NonFlyingBird`.

---

## **I — Interface Segregation (Разделение интерфейсов)**

Класс не зависит от ненужных методов.

```php
interface Worker { function work(); function eat(); }

class Robot implements Worker {
    function work() {} 
    function eat() { } // не нужно → нарушение
}
```

- Правильно:
    

```php
interface Workable { function work(); }
interface Eatable { function eat(); }

class Robot implements Workable { function work() {} }
```

---

## **D — Dependency Inversion (Инверсия зависимостей)**

Классы верхнего уровня зависят от **абстракций, а не конкретных реализаций**.

```php
interface DatabaseInterface { function connect(); }

class MySQLDatabase implements DatabaseInterface { function connect() {} }

class UserRepository {
    private $db;
    public function __construct(DatabaseInterface $db) {
        $this->db = $db;
    }
}
```

- Можно легко заменить `MySQLDatabase` на `PostgreSQLDatabase`.
    

---

**Итог:**  
SOLID помогает писать **чистый, расширяемый и безопасный код**, который легко тестировать и поддерживать.


[Принципы SOLID в картинках / Хабр](https://habr.com/ru/companies/productivity_inside/articles/505430/)

