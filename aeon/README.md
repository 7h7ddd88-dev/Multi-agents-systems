# Aeon Intelligence — Multi-Agent Pain Signal Scanner

## Назначение

Aeon Intelligence — мульти-агентная система для обнаружения потребительских проблем (pain signals) на B2C-рынке. Сканирует 8+ источников, синтезирует сигналы, выявляет продуктовые возможности.

## Органиграмма

```
                    ┌─────────┐
                    │  Board   │
                    │ (Owner)  │
                    └────┬─────┘
                         │
            ┌────────────┼────────────┐
            │            │            │
       ┌────┴────┐  ┌───┴────┐  ┌───┴───────┐
       │  CEO    │  │Research│  │ Creative  │
       │         │  │Analyst │  │ Visionary │
       └────┬────┘  └───┬────┘  └───────────┘
            │            │
     ┌──────┴------┐     │
     │             │     │
┌────┴─────┐ ┌────┴────┐│
│TechProd  │ │Research ││
│Lead      │ │Analyst  ││
└────┬─────┘ └────┬────┘│
     │             │     │
 ┌───┼───┬───┐ ┌──┼──┐  │
 │   │   │   │ │  │  │  │
AS  HN  G2 GT │ Rd TT│  │
    Cp        │  Pt X │
               Tw     │
                      │
            (Research Analyst scanners)
```

## Агенты (15)

### Руководство

| Агент | Роль | Отчитывается перед | Зона ответственности |
|---|---|---|---|
| **CEO** | Стратегия | Board | Синтез insight'ов, продуктовые решения, координация отделов |
| **Creative Visionary** | Идеи | CEO | Нетривиальные продукт-идеи из рыночных сигналов (wild but buildable) |

### Аналитические хабы (синтезаторы)

| Агент | Роль | Отчитывается перед | Зона ответственности |
|---|---|---|---|
| **Tech Product Lead** | Тех-синтез | CEO | Синтез сигналов от HN, App Store, Competitor, Google Trends → block-level отчёты для CEO |
| **Research Analyst** | Контент-синтез | CEO | Синтез сигналов от X/Twitter, TikTok, Reddit, Product Hunt → block-level отчёты для CEO |

### Сканнеры (data collectors)

**Tech Product Lead team** (технологические сигналы):

| Агент | Источник | Выход |
|---|---|---|
| **HN Scanner** | Hacker News (top-200, Ask HN, Show HN) | 50+ pain signals/run |
| **App Store Scanner** | App Store + Google Play (1-3★ отзывы топ-приложений) | 100+ pain signals/run |
| **G2/Review Analyst** | G2, Capterra, Indie Hackers, Quora | Unique pain signals |
| **Google Trends Analyst** | Google Trends (spike >2x/7d, breakout >5000%) | Digital B2C trends |
| **Competitor Analyst** | Product Hunt, G2, Reddit competitor subs, HN | B2C competitor vulnerabilities |

**Research Analyst team** (социальные/контентные сигналы):

| Агент | Источник | Выход |
|---|---|---|
| **X/Twitter Scanner** | X.AI API | 100+ pain signals/run |
| **Reddit Scanner** | 20+ потребительских сабреддитов | 100+ pain signals/run |
| **TikTok Scanner** | TikTok trends (web search indirect) | 50+ pain signals/run |
| **Product Hunt Scanner** | PH launches + comments | 30+ pain signals/run |
| **Consumer Pain Scanner** | Amazon reviews + monetization assessment | Structured pain signals |

### Связь

| Агент | Роль | Отчитывается перед | Зона ответственности |
|---|---|---|---|
| **Jarvis** | Связь с Owner | Board | Escalation к владельцу, Telegram delivery |

## Поток работы

### Scanning Pipeline

```
Phase 1: SCAN (параллельно, 8+ сканнеров)
┌─────────────────────────────────────────────┐
│  HN → ─┐                                    │
│  AS → ─┤                                    │
│  G2 → ─┼──→ Tech Product Lead ──→ CEO       │
│  GT → ─┤       (синтез)                     │
│  CA → ─┘                                    │
│                                              │
│  X/TW → ┐                                   │
│  RD   → ┼──→ Research Analyst ──→ CEO        │
│  TT   → ┤       (синтез)                     │
│  PH   → ┘                                   │
│  CP   → ───────────────────────→ CEO         │
└─────────────────────────────────────────────┘

Phase 2: SYNTHESIS
  Tech Product Lead + Research Analyst → block-level отчёты

Phase 3: CEO DECISION
  CEO → синтез отчётов → продуктовые решения
       → Creative Visionary (опционально: wild ideas)
       → Jarvis → Owner (escalation)

Phase 4: OUTPUT
  Pain signal database → product opportunity backlog
```

### Типичный цикл сканирования

1. **Сканнеры запускаются** (по расписанию или по требованию)
2. Каждый сканнер извлекает **pain signals** из своего источника
3. Сигналы поступают к **хабу-синтезатору** (Tech Product Lead или Research Analyst)
4. Хаб агрегирует, дедуплицирует, ранжирует → **block-level synthesis report**
5. CEO получает отчёты от обоих хабов → **принимает решения**
6. Creative Visionary (опционально) → генерирует **нетривиальные идеи** на основе сигналов
7. Jarvis эскалирует ключевые findings → **Owner через Telegram**

## Структура pain signal

Каждый сканнер выдаёт стандартизированные сигналы:

```
{
  source: "reddit" | "hn" | "app_store" | ...,
  pain_text: "пользователи жалуются на X",
  severity: high | medium | low,
  frequency: N mentions in period,
  product_opportunity: "приложение/сервис который решает это",
  build_time_estimate: "1-4 недели",
  monetization: "freemium | subscription | one-time"
}
```

## Два аналитических хаба

Разделение ответственности по типу сигнала:

| | Tech Product Lead | Research Analyst |
|---|---|---|
| **Источники** | HN, App Store, G2, Google Trends, Competitors | X/Twitter, TikTok, Reddit, Product Hunt |
| **Тип сигнала** | Технологический, продуктовый, конкурентный | Социальный, контентный, трендовый |
| **Выход** | Block-level tech/product report | Block-level social/content report |
| **Фокус** | Что можно построить технически | Чего хотят люди прямо сейчас |

## Ключевые принципы

1. **Сканнеры не синтезируют** — только извлекают сигналы, синтез за хабами
2. **Два хаба = два угла зрения** — техническая осуществимость vs. живой спрос
3. **CEO = финальный синтез** — объединяет оба отчёта в решение
4. **Creative Visionary = опциональный** — подключается когда нужен неочевидный взгляд
5. **Consumer Pain Scanner = гибридный** — Amazon + monetization assessment, идёт напрямую к CEO
6. **Jarvis = мост к Owner** — ключевые findings эскалируются через Telegram
7. **Parallel scanning** — все сканнеры работают параллельно, не блокируют друг друга
