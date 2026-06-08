# SEO Article Generation — Multi-Agent Content System

## Назначение

SEO Article Generation — мульти-агентная система для автоматизированного производства SEO-оптимизированного контента. Полный цикл: от keyword research до локализованной публикации — без участия человека.

## Органиграмма

```
                ┌─────────┐
                │  Board   │
                │ (Owner)  │
                └────┬─────┘
                     │
              ┌──────┴──────┐
              │    CEO      │ (paused — owner direct control)
              └──────┬──────┘
                     │
         ┌───────────┴───────────┐
         │                       │
    ┌────┴─────┐           ┌─────┴────┐
    │   CMO    │           │   CTO    │
    │Marketing │           │Technology│
    └────┬─────┘           └─────┬────┘
         │                       │
    ┌────┼────┬────┐        ┌────┼────┐
    │    │    │    │        │    │    │
   SEO  SMM  Ed.  Loc.   FEnd Back
```

## Агенты (10)

| Агент | Роль | Отчитывается перед | Зона ответственности |
|---|---|---|---|
| **CEO** | Стратегия | Board | Приоритеты, go/no-go, координация отделов. Приостановлен — владелец управляет напрямую |
| **CMO** | Маркетинг | CEO | Стратегия бренда, growth, content operations, делегирование маркетинга |
| **CTO** | Технологии | CEO | Техническая roadmap, архитектура, делегирование разработки. Приостановлен |
| **SEO Strategist** | SEO | CMO | Keyword research, SEO-брифы, метаданные, internal linking, SEO-ревью статей |
| **SMM** | Соцсети | CMO | Стратегия публикации, аудитория, дистрибуция кампаний. Приостановлен |
| **Editor** | Контент | CMO | Редакционный план, черновики, качество копа, координация с маркетинг-кампаниями |
| **Localization** | Переводы | CMO | Локализация статей, культурная адаптация, мультиязычный контент |
| **Frontend** | UI | CTO | Фронтенд-код, UI-реализация, проверка guideline compliance |
| **Backend** | Сервер | CTO | Backend-код, API, БД, серверная логика |
| **Jarvis** | Связь с Owner | Board | Escalation к владельцу, Telegram delivery |

## Поток работы

### Content Pipeline (основной)

```
1. SEO Strategist → keyword research + SEO brief
2. Editor         → draft article by SEO brief
3. SEO Strategist → SEO review (metadata, linking)
4. CMO            → approve / request revision
5. Localization   → translate to target locales
6. SMM            → distribute across channels
```

### Tech Pipeline

```
1. CTO            → define technical task
2. Frontend/Back  → implement code
3. Frontend       → verify UI quality + guidelines
4. CTO            → review + push approved changes
```

## Типы задач

- **SEO brief** — SEO Strategist получает keyword/тему → выдаёт бриф
- **Article** — Editor пишет по брифу → SEO review → CMO approve → publish
- **Localization** — утверждённая статья → перевод на N языков
- **Tech feature** — CTO декомпозирует → Frontend/Backend реализуют

## Ключевые принципы

1. **Editor не публикует** — финальное одобрение за CMO
2. **Frontend не пушит** — код проходит через CTO review
3. **Localization только после approve** — переводится только утверждённый контент
4. **SEO Strategist = quality gate для контента** — статья не идёт дальше без SEO review
5. **Jarvis = мост к Owner** — всё что требует внимания владельца идёт через Jarvis
