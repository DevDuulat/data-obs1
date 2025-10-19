
```bash
 php artisan make:enum StatusType 
```


### Пример класса

```php
<?php

namespace App\Enums;

enum DocumentType: int
{
    case INCOMING = 0;
    case OUTGOING = 1;
    case INTERNAL = 2;

    public function label(): string
    {
        return match ($this) {
            self::INCOMING => 'Входящий документ',
            self::OUTGOING => 'Исходящий документ',
            self::INTERNAL => 'Внутренний документ',
        };
    }
    public function slug(): string
    {
        return match($this) {
            self::INCOMING => 'incoming',
            self::OUTGOING => 'outgoing',
            self::INTERNAL => 'internal',
        };
    }
}




```