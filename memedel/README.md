# MemeDel — Multi-Agent Quant Trading System

## Назначение

MemeDel — мульти-агентная система для количественных исследований и трейдинга мемкоинов (Solana). Полный цикл: генерация гипотез → валидация → бэктест → ревью → деплой.

## Органиграмма

```
              ┌─────────┐
              │  Board   │
              │ (Owner)  │
              └────┬─────┘
                   │
          ┌────────┴────────┐
          │      CEO        │ Координация, приоритеты, go/no-go
          └────────┬────────┘
                   │
              ┌────┴────┐
              │ Product │ Продуктовая валидация, фидбек
              └────┬────┘
                   │
       ┌───────────┼───────────┐
       │           │           │
  ┌────┴────┐ ┌───┴───┐ ┌────┴────┐
  │  Quant  │ │Program│ │Reviewer│
  │  Lead   │ │  mer  │ │   er   │
  └────┬────┘ └───────┘ └────────┘
       │
  ┌────┼────┬────────┐
  │    │    │        │
Back  Sig  Data    (Quant
test  nalR  Anal.   Lead)
er    es.          delegates
```

## Агенты (9)

| Агент | Роль | Отчитывается перед | Зона ответственности |
|---|---|---|---|
| **CEO** | Стратегия | Board | Координация команды, приоритеты, go/no-go решения |
| **Product Lead** | Продукт | CEO | Продуктовая валидация, user feedback, фича go/no-go |
| **Quant Lead** | Исследования | Product | Торговые исследования, гипотезы, стратегический judgment, координация Quant Team |
| **Signal Researcher** | Сигналы | Quant | Генерация гипотез, feature engineering, A/B тесты, signal exploration |
| **Backtester** | Бэктесты | Quant | Бэктесты, risk-adjusted метрики, walk-forward validation, ROC |
| **Data Analyst** | Данные | Quant | Качество данных, пайплайны, артефакт detection, backfill |
| **Programmer** | Разработка | Product | Реализация фич, багфикс, деплой кода |
| **Reviewer** | Ревью | Product | Quality gate — проверяет код Programmer перед деплоем |
| **Jarvis** | Связь с Owner | Board | Escalation к владельцу, Telegram delivery |

## Quant Team — подсистема

Quant Team (Quant Lead + 3 суб-агента) — ядро исследовательского цикла:

```
SignalResearcher ──гипотеза──→ Quant Lead ──приоритизация──→ Backtester
                                      ↑                              │
                                      │         ──результаты────────┘
                                      │
                               DataAnalyst ──чистые данные──→ все
```

### Навыки Quant Team

| Навык | Quant Lead | Backtester | SignalResearcher | DataAnalyst |
|---|:---:|:---:|:---:|:---:|
| quantitative-research | ✅ | ✅ | | |
| risk-management-trading | | ✅ | | |
| scientific-method | ✅ | | ✅ | ✅ |
| statistical-analysis | | | ✅ | ✅ |
| experimental-design | | | ✅ | ✅ |
| monte-carlo | ✅ | ✅ | | |
| tree-of-thoughts | ✅ | | | |

### Адаптации под мемкоины

Мемкоины = жирные хвосты, 5-минутные return'ы, микро-ликвидность, regime shift за часы, zero-price events. Навыки написаны под традиционные рынки — адаптации:

- **Kurtosis > 10** → акцент на Sortino/CVaR вместо Sharpe
- **5-мин бары** вместо дневных, annualize по 5-мин granularity
- **Slippage модель**: 1% (sub-50 SOL), 0.5% (50-200), 0.3% (200+ SOL)
- **Walk-forward**: train=14d, test=3d, step=1d
- **MC симуляции** обязаны включать zero-price absorption state

## Поток работы

### Research Pipeline (основной)

```
1. SignalResearcher → генерирует гипотезу (H0/H1, effect size, pre-registration)
2. Quant Lead       → приоритизирует, одобряет/отклоняет
3. DataAnalyst      → обеспечивает чистые данные (quality check, backfill)
4. Backtester       → walk-forward бэктест + risk-adjusted метрики
5. Quant Lead       → синтезирует результат, принимает решение
6. Product Lead     → продуктовая валидация (стоит ли реализовывать)
7. CEO              → go/no-go
```

### Implementation Pipeline

```
1. Product Lead     → спецификация фичи
2. Programmer       → реализация кода
3. Reviewer         → code review (проверяет соответствие Product + Quant требованиям)
4. Programmer       → исправления по ревью
5. Product Lead     → финальный approve → deploy
```

### Handoff Protocol

Передача задач между агентами — **только через @mention в комментариях**:

```
Quant Lead → Backtester: "@backtester запусти walk-forward для гипотезы MEM-42"
Backtester → Quant Lead: "@quant lead результаты: Sharpe 1.8, Sortino 2.3, CVaR -12%"
Quant Lead → Product: "@product lead гипотеза MEM-42 валидна, рекомендую реализацию"
```

## Типы задач

- **Hypothesis** — SignalResearcher формулирует → Quant приоритизирует → Backtester тестирует
- **Data Quality** — DataAnalyst проверяет/чинит данные для исследования
- **Feature** — Product специфицирует → Programmer пишет → Reviewer проверяет
- **Escalation** — любой агент → Jarvis → Owner (Telegram)

## Ключевые принципы

1. **Quant Lead не делает руками** — делегирует суб-агентам, синтезирует
2. **WakeOnDemand=false** — агенты не просыпаются сами (Hydra incident prevention)
3. **Handoff = @mention** — единственный способ передачи задач
4. **Risk-adjusted метрики обязательны** — WR без σ = слепое пятно
5. **Walk-forward validation** — обязателен для каждой стратегии
6. **Reviewer = quality gate** — код не идёт в прод без Reviewer approve
