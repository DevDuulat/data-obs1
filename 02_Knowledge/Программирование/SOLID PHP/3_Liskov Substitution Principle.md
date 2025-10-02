### Liskov Substitution Principle (LSP)

Когда мы наследуем класс, мы обязуемся соблюдать его контракт, то есть сохранять его поведение. Благодаря этому мы можем подставить новый класс вместо старого — и код, который работает с базовым классом, продолжит работать без изменений.

 ##### Коротко
Наследник обязан соблюдать контракт родителя, поэтому его можно подставить вместо базового класса без поломки кода.


Пример с твоим кодом:
```php
function saveModel(\Illuminate\Database\Eloquent\Model $model) {
    $model->save();
}

saveModel(new \App\Models\Specialist()); // работает
saveModel(new \App\Models\Venue());      // тоже работает
```


И Specialist, и Venue наследуют Model, значит соблюдают один и тот же контракт.

---

В Laravel твой VenueController:
class VenueController extends Controller
{
    public function index(VenueService $venueService) { ... }
    public function show($slug) { ... }
}


Он наследует базовый Controller.

Это значит, что везде, где Laravel ожидает Controller, он может использовать VenueController.

#### Пример корректного соблюдения LSP

Laravel роуты ожидают контроллеры, наследуемые от Controller:

Route::get('/venues', [VenueController::class, 'index']);
Route::get('/venues/{slug}', [VenueController::class, 'show']);


Здесь VenueController заменяет Controller и сохраняет контракт: имеет публичные методы, которые можно вызвать из роутов.

Laravel не ломается, потому что VenueController ведёт себя как контроллер.

#### Пример нарушения LSP

Представь, что ты изменил VenueController так:

```php
class VenueController extends Controller
{
    public function index(VenueService $venueService)
    {
        throw new \Exception("Venues cannot be listed");
    }

    public function show($slug)
    {
        return null; // теперь ничего не возвращает
    }
}
```

#### Теперь:

В роуте Route::get('/venues', [VenueController::class, 'index']); Laravel ждёт корректный ответ (View или Response), но получает исключение.

В show() Laravel ждёт Response, но получает null.

#### Нарушение LSP — VenueController уже не может безопасно подменять базовый Controller.

Пример правильного расширения (LSP соблюдается)

```php

class VenueController extends Controller
{
    public function index(VenueService $venueService)
    {
        $cacheKey = 'venues_public_index_page_' . request('page', 1);

        // Добавили новую логику (логирование), не ломая контракт
        \Log::info("Fetching venues for page " . request('page', 1));

        $venues = Cache::remember($cacheKey, 600, function () use ($venueService) {
            return $venueService->getPaginatedVenues();
        });

        return view('venues.index', compact('venues'));
    }
}

```

Контракт не нарушен: метод всё ещё возвращает Response.
Контроллер можно подставить вместо базового, и всё будет работать.

#### Вывод:
В VenueController сейчас LSP соблюдается, потому что:
он наследует Controller,
возвращает корректные Response для роутов,
расширяет поведение, но не ломает контракт.