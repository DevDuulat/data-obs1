
```php
public function up(): void  
{  
    Schema::create('documents', function (Blueprint $table) {  
        $table->id();  
        $table->string('title');  
        $table->string('slug')->unique();  
        $table->unsignedTinyInteger('document_type');  
        $table->text('comment')->nullable();  
        $table->date('due_date');  
        $table->date('approved_at')->nullable();  
        $table->unsignedTinyInteger('workflow_status');  
        $table->foreignId('user_id')->constrained()->cascadeOnDelete();  
        $table->timestamps();  
    });}
```


---

##### Enum Document Type

| Значение   | Значение по смыслу                                               | Пример                                      |
| ---------- | ---------------------------------------------------------------- | ------------------------------------------- |
| `incoming` | **Входящий документ** — поступил в организацию извне             | Письмо от партнёра, запрос от госоргана     |
| `outgoing` | **Исходящий документ** — создан организацией и отправлен наружу  | Ответное письмо, отчёт, акт                 |
| `internal` | **Внутренний документ** — используется только внутри организации | Приказ, служебная записка, внутренний отчёт |

incoming
outgoing
internal

---
#### Enum workflow_status

draft черновик
in_review в процессе рассмотрения
approved утвержденный
rejected отклоненный
executing Исполнение
completed завершенный
archived  архивированный

---

| Этап         | Статус      | Что происходит                                       |
| ------------ | ----------- | ---------------------------------------------------- |
| Создание     | `draft`     | Автор готовит документ                               |
| Согласование | `in_review` | Руководители или коллеги проверяют                   |
| Утверждение  | `approved`  | Документ утверждён (официально принят)               |
| Исполнение   | `executing` | По документу выполняются поручения, задачи, действия |
| Завершение   | `completed` | Все действия по документу выполнены                  |
| Отказ        | `rejected`  | Документ отклонён на любом этапе                     |
| Архивация    | `archived`  | Документ закрыт и помещён в архив                    |


---

#### Enum status
pending в ожидании
approved  утвержденный
rejected отклоненный

---

#### Enum Role
approver  Утверждающий
final_approver Окончательный утверждающий
reviewer  Проверяющий

---




id
user_id fk
document_id fk
token_limit
access_token uuid
