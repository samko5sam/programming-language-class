---
marp: true
theme: rose-pine-dawn
class: blue
paginate: true
---

# 聯想猜猜看 - AI 機器人小遊戲
##### [🔗 0313_AIGame.ipynb](https://github.com/samko5sam/programming-language-class/blob/main/0313_AIGame.ipynb)

---

# AI 遊戲簡介

一位玩家描述他看到的題目，另一位玩家負責猜出答案，只要猜對答案就可以加一分，兩位玩家輪流進行描述，最後看誰的分數比較高。

---

# 特別的程式碼
- **取得題目組**：
  - 透過試算表 API 取得題目後進行整理
  - 隨機選擇一個題目，紀錄出過的題目減少重複的機會

  ```python
  question = random.choice(all_questions)

  while question in past_questions:
      if len(past_questions) == len(all_questions):
          past_questions = []
      question = random.choice(all_questions)
  past_questions.append(question)
  ```

---

# Prompt 怎麼下
- **描述者的 Prompt**：

  > 請以不清楚的一小句話描述題目'{question}'，不講出題目中的字，不重複講過的描述

  - 例：題目「擴音機」 → 「能夠讓聲音變得更大的設備。」
- **猜題者的 Prompt**：
  > 請根據描述猜出題目：能夠讓聲音變得更大的設備。 回答請簡明，不要重複猜過的答案，不要標點符號，使用台灣用語

---

# 遊戲怎麼進行
- **規則**：
  1. 兩位玩家（AI）輪流描述與猜測
  2. 描述者給提示，猜測者回答
  3. 猜對 +1 分，猜錯換人
- **流程**：
  - 第 1 輪：AI 1 描述 → AI 2 猜
  - 第 2 輪：AI 2 描述 → AI 1 猜
  - AI 間進行討論對話

---

# 使用 AI 模型
- **使用模型**：
  - Google Gemini
  - OpenAI GPT-4o-mini
- **實作**：
```python
# Gemini

def answerGemini(prompt):
    response = model.generate_content(prompt)
    return response.text.strip()

```

---

<div style="display: flex; column-gap: 10px;">

<div style="flex: 1;">

##### 結果範例 - 第 1 輪
```
AI 1 描述：
「外酥內嫩的油炸肉片，常佐以各式調味」
AI 2 回答：炸豬排
「一種脆皮肉片，常伴隨香料和調味醬」
AI 2 回答：豬抓皮
結果：
答案：雞排
AI 2 沒猜對，換人！
```

</div>

<div style="flex: 1;">

##### 結果範例 - 第 2 輪
```
AI 2 描述：
「一個盛裝液體的容器，通常有圓形的底部和把手」
AI 1 回答：水壺
「一個用來盛裝液體的容器」
AI 1 回答：杯子
結果：
答案：水桶
AI 1 沒猜對！
```

</div>

</div>

---

##### 計分展示
```
目前分數：
AI 1：0 分
AI 2：0 分
兩輪後無人得分，遊戲繼續！
```