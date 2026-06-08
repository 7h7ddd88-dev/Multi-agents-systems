# Multi-Agent Systems

Схематическая документация по трём независимым мульти-агентным системам, работающим на платформе Paperclip.

## Системы

| Система | Агентов | Домен | Ключевой паттерн |
|---|---|---|---|
| **[SEO Article Generation](seo-article-generation/)** | 10 | Контент & SEO | Конвейер: SEO brief → Article → Review → Publish → Localize |
| **[MemeDel](memedel/)** | 9 | Quant Trading | Исследовательский цикл: Hypothesis → Validate → Backtest → Review → Deploy |
| **[Aeon Intelligence](aeon/)** | 15 | Pain Signal Discovery | Сканнеры → Синтезаторы → CEO → Product Opportunities |

## Общая архитектура

Все три системы работают на **Paperclip** — платформе мульти-агентной оркестрации.

### Базовые принципы

1. **Каждый агент = одна зона ответственности** — чётко определённые boundaries
2. **Иерархия отчётности** — каждый агент отчитывается перед конкретным руководителем
3. **Handoff через @mention** — единственный способ передачи задач между агентами
4. **Jarvis = мост к Owner** — во всех трёх системах Jarvis эскалирует к владельцу через Telegram
5. **No auto-recovery** — агенты НЕ создают recovery-задачи (предотвращение Hydra-цепочек)

### Паттерны оркестрации

**SEO Article Generation — Pipeline (линейный конвейер):**
```
SEO → Editor → Review → CMO approve → Localize → SMM distribute
```
Задачи идут по линейному пайплайну с approval gates.

**MemeDel — Research Loop (исследовательский цикл):**
```
Signal → Quant prioritize → Backtest → Quant synthesize → Product validate → Build
```
Циклический процесс с feedback loops и количественной валидацией.

**Aeon — Fan-out/Fan-in (веерный сбор и синтез):**
```
8 Scanners (parallel) → 2 Hubs (aggregate) → CEO (synthesize) → Owner
```
Параллельный сбор → агрегация → синтез → решение.

## Платформа: Paperclip

Paperclip — платформа мульти-агентной оркестрации, на которой работают все три системы.

### Ключевые механизмы

- **Issues** — задачи, которые агенты подхватывают и выполняют
- **Comments + @mention** — handoff и коммуникация между агентами
- **Heartbeat** — периодическая проверка (по требованию или по расписанию)
- **wakeOnDemand** — агент просыпается только при назначении задачи (не автоматически)
- **Status flow** — `todo → in_progress → done` (с blocked / reviewing состояниями)

## Структура репозитория

```
├── README.md                          ← этот файл
├── seo-article-generation/
│   └── README.md                      ← SEO Article Generation: агенты, pipeline, принципы
├── memedel/
│   └── README.md                      ← MemeDel: Quant Team, research loop, навыки
└── aeon/
    └── README.md                      ← Aeon: сканнеры, синтезаторы, pain signals
```

## Для кого

Эта документация создана для оценки технической экспертизы команды при подаче на грант. Показывает:
- Способность проектировать мульти-агентные системы с чётким разделением ответственности
- Понимание различных паттернов оркестрации (pipeline, research loop, fan-out/fan-in)
- Опыт работы с LLM-агентами в продакшн-контуре
- Практику автономной оркестрации через единую платформу
