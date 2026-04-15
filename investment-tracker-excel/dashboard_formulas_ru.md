# Формулы для листа Dashboard (таблица `tDash`)

Ниже формулы даны с разделителем `;` для русской локали Excel.

## Формулы по колонкам `tDash`

### QtyOpen
```excel
=СУММЕСЛИМН(tLots[QtyOpen];tLots[Account];[@Account];tLots[Ticker];[@Ticker])
```

### CostOpen
```excel
=СУММЕСЛИМН(tLots[CostOpen];tLots[Account];[@Account];tLots[Ticker];[@Ticker])
```

### AvgBuyOpen
```excel
=ЕСЛИОШИБКА([@CostOpen]/[@QtyOpen];0)
```

### CouponGross
```excel
=СУММЕСЛИМН(tOps[AmountGross];tOps[Account];[@Account];tOps[Ticker];[@Ticker];tOps[OpType];"COUPON")
```

### DividendGross
```excel
=СУММЕСЛИМН(tOps[AmountGross];tOps[Account];[@Account];tOps[Ticker];[@Ticker];tOps[OpType];"DIVIDEND")
```

### TaxTotal
```excel
=СУММЕСЛИМН(tOps[AmountGross];tOps[Account];[@Account];tOps[Ticker];[@Ticker];tOps[OpType];"TAX")
```

### IncomeNet
```excel
=[@CouponGross]+[@DividendGross]-[@TaxTotal]
```

### RealizedPnL_Sell
```excel
=СУММЕСЛИМН(tClos[PnL];tClos[Account];[@Account];tClos[Ticker];[@Ticker];tClos[CloseType];"SELL")
```

### RealizedPnL_Redemption
```excel
=СУММЕСЛИМН(tClos[PnL];tClos[Account];[@Account];tClos[Ticker];[@Ticker];tClos[CloseType];"REDEMPTION")
```

### BPIF_GrowthSold_RUB
```excel
=ЕСЛИ([@InstrType]="BPIF";[@RealizedPnL_Sell];0)
```

### BPIF_GrowthSold_Pct
```excel
=ЕСЛИ([@InstrType]<>"BPIF";0;ЕСЛИОШИБКА([@BPIF_GrowthSold_RUB]/СУММЕСЛИМН(tClos[CostClosed];tClos[Account];[@Account];tClos[Ticker];[@Ticker];tClos[CloseType];"SELL");0))
```

### MarketValue (последнее значение по дате из `tSnap`)
```excel
=ЕСЛИОШИБКА(ПРОСМОТР(2;1/((tSnap[Account]=[@Account])*(tSnap[Ticker]=[@Ticker]));tSnap[MarketValue]);0)
```

### BPIF_GrowthCurrent_RUB
```excel
=ЕСЛИ([@InstrType]="BPIF";[@MarketValue]-[@CostOpen];0)
```

### BPIF_GrowthCurrent_Pct
```excel
=ЕСЛИ([@InstrType]<>"BPIF";0;ЕСЛИОШИБКА([@BPIF_GrowthCurrent_RUB]/[@CostOpen];0))
```

## Блок План/Факт по месяцам

Создай рядом мини-таблицу, например `tMonth`, с колонками:
- `MonthKey` (формат `yyyy-mm`)
- `PlanCoupon`
- `FactCoupon`
- `PlanDividend`
- `FactDividend`
- `TotalPlan`
- `TotalFact`

### PlanCoupon
```excel
=СУММПРОИЗВ((ТЕКСТ(tPlan[PlanDate];"гггг-мм")=[@MonthKey])*(tPlan[PayType]="COUPON")*(tPlan[PlannedGross]))
```

### FactCoupon
```excel
=СУММПРОИЗВ((ТЕКСТ(tOps[Date];"гггг-мм")=[@MonthKey])*(tOps[OpType]="COUPON")*(tOps[AmountGross]))
```

### PlanDividend
```excel
=СУММПРОИЗВ((ТЕКСТ(tPlan[PlanDate];"гггг-мм")=[@MonthKey])*(tPlan[PayType]="DIVIDEND")*(tPlan[PlannedGross]))
```

### FactDividend
```excel
=СУММПРОИЗВ((ТЕКСТ(tOps[Date];"гггг-мм")=[@MonthKey])*(tOps[OpType]="DIVIDEND")*(tOps[AmountGross]))
```

### TotalPlan
```excel
=[@PlanCoupon]+[@PlanDividend]
```

### TotalFact
```excel
=[@FactCoupon]+[@FactDividend]
```

## Примечание по локализации

Если функции не распознаются, используй английские аналоги:
- `СУММЕСЛИМН` -> `SUMIFS`
- `ЕСЛИОШИБКА` -> `IFERROR`
- `ПРОСМОТР` -> `LOOKUP`
- `СУММПРОИЗВ` -> `SUMPRODUCT`
- `ТЕКСТ` -> `TEXT`
