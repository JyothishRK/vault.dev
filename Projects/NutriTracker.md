---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Hi {{first_name}},

Here’s your personalized meal plan for today.

🎯 Daily Goal
Calories: 2,000 kcal
Diet: Vegetarian
Protein Target: 120g

⸻

🍳 Breakfast (8:00 AM)
Vegetable Oats Upma

* Oats (80g)
* Mixed vegetables (100g)
* Curd (100g)

Calories: 450 kcal
Protein: 20g

⸻

🥗 Lunch (1:00 PM)
Dal Rice Bowl

* Brown rice (150g)
* Dal (200g)
* Mixed vegetable salad

Calories: 650 kcal
Protein: 30g

⸻

☕ Evening Snack (5:00 PM)
Protein Smoothie

* Milk (250ml)
* Banana (1)
* Peanut butter (15g)

Calories: 300 kcal
Protein: 20g

⸻

🍽️ Dinner (8:00 PM)
Paneer & Roti

* Paneer curry (150g)
* Whole wheat roti (3)

Calories: 600 kcal
Protein: 50g

⸻

📊 Daily Summary
Calories: 2,000 kcal
Protein: 120g

💡 Tip of the Day
Drink at least 2.5 liters of water and try to walk for 20 minutes after dinner.

Have a healthy day!

— Team NutriSathi ^UFN2tLrx

Models:

* users
* user_preferences
* recipes
* meal_plans
* delivery_logs

 ^doYZAstf

1. users

Authentication and account-related data only.

users

Column        Type
id        UUID
name        VARCHAR
email        VARCHAR UNIQUE
password_hash        VARCHAR
email_verified        BOOLEAN
status        VARCHAR
created_at        TIMESTAMP
updated_at        TIMESTAMP

Example:

{
  "id": "uuid",
  "name": "Jyothish",
  "email": "jyothish@gmail.com",
  "emailVerified": true,
  "status": "active"
}

⸻

2. user_preferences

Nutrition and delivery configuration.

user_preferences

Column        Type
id        UUID
user_id        UUID FK UNIQUE
goal        VARCHAR
calories        INTEGER
protein        INTEGER
carbs        INTEGER
fat        INTEGER
meal_count        INTEGER
diet_type        VARCHAR
send_time        TIME
timezone        VARCHAR
delivery_channels        JSONB
created_at        TIMESTAMP
updated_at        TIMESTAMP

Example:

{
  "userId": "uuid",
  "goal": "fat_loss",
  "calories": 2200,
  "protein": 140,
  "carbs": 250,
  "fat": 60,
  "mealCount": 4,
  "dietType": "veg",
  "sendTime": "08:00",
  "deliveryChannels": ["email"]
} ^KUwkl1cf

3. recipes

Master recipe catalog.

recipes

Column        Type
id        UUID
name        VARCHAR
meal_type        VARCHAR
diet_type        VARCHAR
calories        INTEGER
protein        INTEGER
carbs        INTEGER
fat        INTEGER
ingredients        JSONB
instructions        JSONB
active        BOOLEAN
created_at        TIMESTAMP

⸻

4. meal_plans

Store generated plans as snapshots.

meal_plans

Column        Type
id        UUID
user_id        UUID FK
plan_date        DATE
total_calories        INTEGER
meals        JSONB
generated_at        TIMESTAMP

Example:

{
  "breakfast": {
    "name": "Oats Upma",
    "calories": 400
  },
  "lunch": {
    "name": "Rajma Rice",
    "calories": 700
  }
}

⸻

5. delivery_logs

Tracks delivery status.

delivery_logs

Column        Type
id        UUID
user_id        UUID FK
meal_plan_id        UUID FK
channel        VARCHAR
status        VARCHAR
sent_at        TIMESTAMP
error_message        TEXT

Status examples:

pending
sent
failed
 ^QHHZ900b

⸻

Relationships

users
  |
  | 1:1
  |
user_preferences
users
  |
  | 1:N
  |
meal_plans
users
  |
  | 1:N
  |
delivery_logs
meal_plans
  |
  | 1:N
  |
delivery_logs
 ^0RyMh12c

%%
## Drawing
```compressed-json
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=
```
%%