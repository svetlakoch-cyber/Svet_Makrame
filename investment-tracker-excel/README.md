# Инвестиционный учет в Excel

Готовый шаблон проекта для учета:
- сделок по акциям, облигациям и БПИФ,
- лотного учета (FIFO),
- погашений облигаций,
- купонов и дивидендов (грязными, налог отдельной строкой),
- сводки план/факт по месяцам,
- результата по БПИФ (текущий и реализованный).

## Что внутри

- `ops_template.csv` — журнал операций (`Ops`).
- `lots_template.csv` — шаблон лотов (`Lots`).
- `closures_template.csv` — шаблон закрытий лотов FIFO (`Closures`).
- `plan_template.csv` — план выплат (`Plan`).
- `snapshot_template.csv` — срез рыночной стоимости (`Snapshot`).
- `dashboard_template.csv` — таблица сводки по инструментам (`Dashboard`).
- `dashboard_formulas_ru.md` — готовые формулы для Excel.

## Как использовать

1. Создай файл `Portfolio.xlsx`.
2. Импортируй каждый `*_template.csv` на отдельный лист:
   - `Ops`, `Lots`, `Closures`, `Plan`, `Snapshot`, `Dashboard`.
3. На каждом листе преобразуй диапазон в таблицу (`Ctrl + T`) и дай имена:
   - `tOps`, `tLots`, `tClos`, `tPlan`, `tSnap`, `tDash`.
4. На листе `Dashboard` вставь формулы из `dashboard_formulas_ru.md`.
5. Заполняй `Ops`, `Plan`, `Snapshot`; таблицы `Lots` и `Closures` веди по FIFO.

## Важные правила учета

- Счета `IIS` и `BROKER` учитывать раздельно.
- Налог вносить отдельной строкой `OpType = TAX` в день выплаты.
- Для облигаций без оферты использовать `REDEMPTION` при погашении.
- Для БПИФ:
  - текущий результат = `MarketValue - CostOpen`,
  - реализованный результат = сумма `PnL` по `CloseType = SELL`.
