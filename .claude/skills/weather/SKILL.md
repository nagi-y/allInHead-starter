---
name: weather
description: "天気情報の取得 — 指定した場所の現在の天気と週間予報を取得する。「天気は?」「今日寒い?」「傘いる?」「週末の天気教えて」など天気に関する質問に使う。APIキー不要"
user-invocable: true
disable-model-invocation: false
allowed-tools: Bash(curl *)
---

# /weather — 天気情報取得

指定した場所の天気を wttr.in と Open-Meteo から取得する。APIキー不要。

## 使い方

```
/weather             # デフォルト（ユーザーの場所。world/about-you.md を参照）
/weather 東京
/weather Tokyo
```

---

## 手順

### 1. 場所を決める

`$ARGUMENTS` が場所名。空なら `world/about-you.md` から場所を探す。それもなければユーザーに聞く。

### 2. 現在の天気（wttr.in）

```bash
curl -s "wttr.in/{場所}?format=4&lang=ja" 2>/dev/null
```

失敗したら英語でも試す。

### 3. 週間予報（Open-Meteo）

```bash
# ジオコーディング（場所名 → 緯度経度）
curl -s "https://geocoding-api.open-meteo.com/v1/search?name={場所}&count=1&language=ja&format=json"

# 週間予報（7日間）
curl -s "https://api.open-meteo.com/v1/forecast?latitude={lat}&longitude={lon}&daily=temperature_2m_max,temperature_2m_min,precipitation_probability_max,weathercode&timezone=Asia/Tokyo&forecast_days=7"
```

### 4. 結果をまとめて表示

天気コード（WMO）の簡易マッピング:
- 0: 晴れ
- 1-3: 曇り
- 51-67: 雨
- 71-77: 雪
- 80-82: にわか雨
- 95-99: 雷雨
