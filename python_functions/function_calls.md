# Python 函式呼叫教學：參數、位置引數與關鍵字引數

在 Python 程式設計中，「函式（Function）」是將一段可重複使用的程式邏輯打包的重要機制。要正確且靈活地使用函式，理解**參數（Parameters）**與**引數（Arguments）**的傳遞方式是初學者最重要的基本功。

---

## 1. 參數（Parameter）與 引數（Argument）的差別

許多人在初學時容易混淆這兩個詞，但它們在概念上有著清楚的區別：

| 專有名詞 | 英文術語 | 出現地點 | 說明 |
| :--- | :--- | :--- | :--- |
| **參數** (形參) | **Parameter** | 函式**定義**時 | 函式宣告括號內的變數名稱，用來接收資料的「佔位符」。 |
| **引數** (實參) | **Argument** | 函式**呼叫**時 | 實際傳入函式內供運算使用的「具體數值或物件」。 |

### 程式碼示意：
```python
# 這裡的 name 和 message 是「參數 (Parameters)」
def greet(name, message):
    print(f"[{name}]: {message}")

# 這裡傳入的 "Alice" 和 "早安" 則是「引數 (Arguments)」
greet("Alice", "早安")
```

> **記憶口訣：**
> * **參數 (Parameter)** 是定義時預留的「坑位」。
> * **引數 (Argument)** 是呼叫時實際填進去的「蘿蔔」。

---

## 2. 位置引數 (Positional Arguments)

**位置引數**是 Python 最常見也最基本的引數傳遞方式。

### 核心原則
* 呼叫函式時，引數會**依照順序（由左至右）**對應到函式定義的參數上。
* 引數的「位置」決定了它會被指派給哪一個參數。

### 範例：
```python
def make_intro(name, age, city):
    print(f"大家好，我是 {name}，今年 {age} 歲，住在 {city}。")

# 依照順序傳入：name="小明", age=25, city="台北"
make_intro("小明", 25, "台北")
# 輸出：大家好，我是 小明，今年 25 歲，住在 台北。

# 若順序顛倒，數值對應就會出錯：
make_intro("台北", "小明", 25)
# 輸出：大家好，我是 台北，今年 小明 歲，住在 25。
```

### 常見錯誤：引數數量不符
位置引數的數量必須與函式所定義的必填參數完全一致：
```python
# 缺少引數：TypeError: make_intro() missing 1 required positional argument: 'city'
# make_intro("小明", 25)

# 多給引數：TypeError: make_intro() takes 3 positional arguments but 4 were given
# make_intro("小明", 25, "台北", "學生")
```

---

## 3. 關鍵字引數 (Keyword Arguments)

呼叫函式時，直接指定 `參數名稱 = 數值` 的語法稱為**關鍵字引數**。

### 核心優勢
1. **打破順序限制**：因為明確指名參數名稱，引數出現的順序不再重要。
2. **提高程式可讀性**：看到呼叫端就能立刻知道每個值代表的意義，尤其在參數很多時特別清晰。

### 範例：
```python
def send_email(to, subject, content):
    print(f"收件人: {to}\n主旨: {subject}\n內文: {content}")

# 使用關鍵字引數呼叫（可自由變換順序）
send_email(
    subject="會議通知",
    to="alice@example.com",
    content="明天上午十點舉行專案進度會議。"
)
```

---

## 4. 位置引數與關鍵字引數混用規則

Python 允許在同一函式呼叫中混合使用位置引數與關鍵字引數，但必須遵守以下嚴格規則：

> [!IMPORTANT]
> **黃金法則：所有的位置引數，必須放在任何關鍵字引數之前！**

### 正確示範：
```python
def create_profile(name, age, country, occupation):
    print(f"{name} ({age}歲) - 來自 {country}，職業是 {occupation}")

# 前兩個用位置引數，後兩個用關鍵字引數（合法）
create_profile("Bob", 30, country="日本", occupation="工程師")
```

### 常見錯誤 1：位置引數放在關鍵字引數後面 (`SyntaxError`)
```python
# 語法錯誤：positional argument follows keyword argument
# create_profile(name="Bob", 30, "日本", occupation="工程師")
```

### 常見錯誤 2：重複賦值給同一個參數 (`TypeError`)
```python
# 錯誤：name 已經透過位置引數指定為 "Bob"，又透過關鍵字引數給了一次值
# TypeError: create_profile() got multiple values for argument 'name'
# create_profile("Bob", 30, "日本", "工程師", name="Charlie")
```

---

## 5. 預設參數值 (Default Parameter Values)

在定義函式時，可以給參數設定**預設值**。若呼叫時省略該引數，函式就會自動採用預設值。

### 語法範例：
```python
# tax_rate 預設為 0.05
def calculate_total(price, quantity, tax_rate=0.05):
    subtotal = price * quantity
    total = round(subtotal * (1 + tax_rate), 2)
    return total

# 1. 不傳 tax_rate，使用預設值 0.05
print(calculate_total(100, 2))  # 輸出: 210.0

# 2. 透過位置引數覆寫預設值
print(calculate_total(100, 2, 0.1))  # 輸出: 220.0

# 3. 透過關鍵字引數覆寫預設值
print(calculate_total(100, 2, tax_rate=0.08))  # 輸出: 216.0
```

> [!WARNING]
> **注意事項：**
> 在定義函式時，**有預設值的參數必須排在沒有預設值的參數後面**。
> 例如：`def func(a=1, b):` 會直接引發 `SyntaxError: non-default argument follows default argument`。

---

## 6. 進階延伸：不定長度引數 (`*args` 與 `**kwargs`)

當你不確定呼叫端會傳入多少個引數時，可以使用星號語法：

1. **`*args`（任意數量位置引數）：** 將多個位置引數打包成一個 **Tuple（元組）**。
2. **`**kwargs`（任意數量關鍵字引數）：** 將多個關鍵字引數打包成一個 **Dictionary（字典）**。

```python
def log_event(event_name, *tags, **details):
    print(f"事件: {event_name}")
    print(f"標籤 (tuple): {tags}")
    print(f"詳細資訊 (dict): {details}")

log_event(
    "使用者登入",
    "安全性", "Web端",       # 會被收集進 tags
    ip="192.168.1.1",        # 會被收集進 details
    browser="Chrome"
)
```

---

## 7. 隨堂測驗與自我練習

### 題目一：辨別合法呼叫
請觀察以下函式定義：
```python
def order_coffee(size, flavor="拿鐵", iced=True):
    temp = "冰" if iced else "熱"
    return f"{temp}{size}{flavor}"
```
下列呼叫方式中，哪幾項是**合法**的？哪幾項會報錯？
1. `order_coffee("大杯")`
2. `order_coffee(flavor="美式", "中杯")`
3. `order_coffee("小杯", iced=False)`
4. `order_coffee("特大杯", flavor="卡布奇諾", iced=False)`
5. `order_coffee(size="大杯", "拿鐵")`

<details>
<summary>點擊展開解答與說明</summary>

* **選項 1 合法：** `size="大杯"`，其餘使用預設值。結果為「冰大杯拿鐵」。
* **選項 2 錯誤：** 引發 `SyntaxError`，位置引數 `"中杯"` 不可放在關鍵字引數 `flavor="美式"` 之後。
* **選項 3 合法：** 第一個為位置引數，第三個以關鍵字引數覆寫，`flavor` 使用預設值。結果為「熱小杯拿鐵」。
* **選項 4 合法：** 第一個為位置引數，其餘兩個為關鍵字引數。結果為「熱特大杯卡布奇諾」。
* **選項 5 錯誤：** 引發 `SyntaxError`，位置引數 `"拿鐵"` 出現在關鍵字引數之後。
</details>

---

### 題目二：動手實作除錯題
小華寫了一個計算購物車折扣的函式：
```python
def apply_discount(item, original_price, discount=0.9, vip_only=False):
    final_price = original_price * discount
    return f"品項: {item}, 折扣價: {final_price:.0f}, VIP專屬: {vip_only}"
```
請嘗試用**關鍵字引數**的方式呼叫此函式，要求：
* `item` 為 `"機械鍵盤"`
* `original_price` 為 `3000`
* `vip_only` 設定為 `True`
* `discount` 保留原本的預設值不傳

<details>
<summary>點擊展開解答</summary>

```python
result = apply_discount(
    item="機械鍵盤",
    original_price=3000,
    vip_only=True
)
print(result)
# 輸出：品項: 機械鍵盤, 折扣價: 2700, VIP專屬: True
```
</details>
