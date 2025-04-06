# Руководство по использованию технических индикаторов
- Для комментариев используй #, как в Python

## Общая информация
- Результат всегда должен быть присвоен переменной `result`
- Можно использовать несколько строк кода, разделяя их переносом строки
## Доступные таймфреймы
Для всех индикаторов можно использовать следующие таймфреймы:
- '1m' - 1 минута
- '3m' - 3 минуты
- '5m' - 5 минут
- '15m' - 15 минут (используется по умолчанию в большинстве индикаторов)
- '30m' - 30 минут
- '1h' - 1 час
- '2h' - 2 часа
- '4h' - 4 часа
- '6h' - 6 часов
- '12h' - 12 часов
- '1d' - 1 день
- '1w' - 1 неделя
- '1M' - 1 месяц

## Типы выражений

### 1. Фильтрация
- Применяется первым
- Используется для первичного отбора торговых пар
- Результат должен быть логическим (true/false)
```javascript
// Пример фильтрации по объему
dailyVolume = DAILYVOL()
result = dailyVolume > 1000000

// Пример фильтрации по RSI
result = RSI() < 30 and RSI('4h') < 35
```

### 2. Сортировка
- Определяет приоритет торговых пар
- Результат должен быть числовым
- Сортировка всегда от большего к меньшему
```javascript
// Сортировка по волатильности
volatility = VOLATILITY('max')
result = volatility

// Сортировка по отклонению от Bollinger Bands
price = PRICE()
bb_lower = BB('15m', 14, 'lower')
result = (price - bb_lower) / price * 100
```

### 3. Выражение входа
- Применяется последним
- Определяет точный момент входа
- Результат должен быть логическим (true/false)
```javascript
// Вход по пересечению MA
sma_fast = SMA('15m', 'close', 8)
sma_slow = SMA('15m', 'close', 21)
result = sma_fast > sma_slow and RSI() < 40

// Вход по Bollinger Bands
result = PRICE() < BB('15m', 14, 'lower') and RSI() < 30
```

## Доступные индикаторы

### RSI (Relative Strength Index)
```typescript
RSI(timeframe?, period?, price?, index?)
// timeframe: '15m' (по умолчанию), '1h', '4h', '1d' и т.д.
// period: 14 (по умолчанию)
// price: 'close' (по умолчанию), 'open', 'high', 'low'
// index: 0 (по умолчанию) - смещение назад по времени
```

### CCI (Commodity Channel Index)
```typescript
CCI(timeframe?, period?, index?)
// timeframe: '15m' (по умолчанию)
// period: 14 (по умолчанию)
// index: 0 (по умолчанию)
```

### PRICE (Получение цены)
```typescript
PRICE(timeframe?, price?, index?)
// timeframe: '15m' (по умолчанию)
// price: 'close' (по умолчанию), 'open', 'high', 'low'
// index: 0 (по умолчанию)
```

### BB (Bollinger Bands)
```typescript
BB(timeframe?, period?, line?, price?, stdDev?, index?)
// timeframe: '15m' (по умолчанию)
// period: 14 (по умолчанию)
// line: 'upper' (по умолчанию), 'middle', 'lower'
// price: 'close' (по умолчанию)
// stdDev: 2 (по умолчанию)
// index: 0 (по умолчанию)
```

### MACD
```typescript
MACD(timeframe?, line?, price?, fastPeriod?, slowPeriod?, signalPeriod?, SimpleMAOscillator?, SimpleMASignal?, index?)
// timeframe: '15m' (по умолчанию)
// line: 'MACD' (по умолчанию), 'signal', 'histogram'
// price: 'close' (по умолчанию)
// fastPeriod: 5 (по умолчанию)
// slowPeriod: 8 (по умолчанию)
// signalPeriod: 3 (по умолчанию)
// SimpleMAOscillator: false (по умолчанию)
// SimpleMASignal: false (по умолчанию)
// index: 0 (по умолчанию)
```

### Скользящие средние (SMA, EMA, WMA)
```typescript
SMA(timeframe?, price?, period?, index?)
EMA(timeframe?, price?, period?, index?)
WMA(timeframe?, price?, period?, index?)
// timeframe: '15m' (по умолчанию)
// price: 'close' (по умолчанию)
// period: 8 (по умолчанию)
// index: 0 (по умолчанию)
```

### Дополнительные индикаторы
- ADL(timeframe?, index?)
- ATR(timeframe?, period?, index?)
- AO(timeframe?, fastPeriod?, slowPeriod?, index?)
- FI(timeframe?, period?, index?)
- MFI(timeframe?, period?, index?)
- OBV(timeframe?, index?)
- PSAR(timeframe?, step?, max?, index?)
- ROC(timeframe?, price?, period?, index?)
- STOCH(timeframe?, line?, period?, signalPeriod?, index?)

### MRI (Mean Reversion Channel)
```typescript
MRI(value?, timeframe?, source?, type?, length?, innerMult?, outerMult?, index?)
// value: 'meanLine' (по умолчанию), 'meanRange', 'upperBand1', 'lowerBand1', 'upperBand2', 'lowerBand2', 'condition'
// timeframe: '15m' (по умолчанию)
// source: 'close' (по умолчанию), 'open', 'high', 'low'
// type: 'SuperSmoother' (по умолчанию), 'Ehlers EMA', 'Gaussian', 'Butterworth', 'BandStop', 'SMA', 'EMA', 'RMA'
// length: 200 (по умолчанию)
// innerMult: 1.0 (по умолчанию)
// outerMult: 2.415 (по умолчанию)
// index: 0 (по умолчанию)

// Значения condition:
// 1-3: перекупленность (слабая, средняя, сильная)
// 4: цена около средней линии
// 5: цена выше средней линии
// -1 до -3: перепроданность (слабая, средняя, сильная)
// -4: цена около средней линии
// -5: цена ниже средней линии
```

### Специальные индикаторы
```typescript
// Дневной объем
DAILYVOL()

// Волатильность
VOLATILITY(percent, timeframe?, length?)
// percent: процент изменения цены для фильтрации (обязательный параметр)
// timeframe: '1h' (по умолчанию)
// length: 24 (по умолчанию)
// Возвращает количество свечей, где изменение цены (high-low) >= percent%

// Pump/Dump
PUMPDUMP(type?, timeframe?, length?)
// type: 'min' (по умолчанию), 'max'
// timeframe: '1h' (по умолчанию)
// length: 24 (по умолчанию)
// Возвращает минимальное или максимальное изменение цены в % за указанный период
```

## Примеры полных стратегий

### Фильтрация по объему и волатильности
```javascript
vol = DAILYVOL()
volatility = VOLATILITY(2, '15m', 100)  # Количество свечей с изменением цены >= 2%
result = vol > 1000000 and volatility > 10  # Если больше 10 свечей с высокой волатильностью
```

### Сортировка по отклонению от SMA
```javascript
price = PRICE()
sma = SMA('15m', 'close', 20)
result = abs((price - sma) / sma * 100)
```

### Вход по RSI и Bollinger Bands
```javascript
rsi = RSI()
price = PRICE()
bb_lower = BB('15m', 14, 'lower')
result = rsi < 30 and price < bb_lower
```

### Фильтрация по Pump/Dump
```javascript
pumpDump = PUMPDUMP('max', '1h', 24)  # Максимальное изменение цены за последние 24 часа
result = pumpDump < 5  # Исключаем пары с резкими движениями цены
```

## Важные замечания
1. Все временные рамки должны соответствовать формату биржи
2. Индексы позволяют получать исторические значения (0 - текущее, 1 - предыдущее и т.д.)
3. При работе с ценами доступны: 'open', 'high', 'low', 'close'
4. Все функции возвращают false в случае ошибки или отсутствия данных
5. В выражениях сортировки нельзя использовать блоки, только прямой код
6. Результат всегда должен быть присвоен переменной `result`
7. Для комментариев используй #, как в Python
