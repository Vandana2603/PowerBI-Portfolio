# Pizza Sales Data Analysis Project

## PART 1: Connecting & Shaping Data

### 📌 Tasks
Before proceeding, try answering the following:

✅ How would you load and clean data from the given CSV files in Power BI?  
✅ How would you ensure that all tables have the correct data types?  
✅ How would you handle missing or null values in the datasets?  
✅ How would you create a new column that extracts only the hour from the "time" field in the `orders` table?  
✅ How would you create a new column in `pizza_types` that extracts only the first ingredient from the ingredient list?

<details>
  <summary>💡 Show Solutions</summary>

- **Load Data:** Connect to all CSV files in Power BI and promote headers.
- **Ensure Correct Data Types:**  
  - `order_id`, `order_details_id`, `pizza_id` → Whole Numbers  
  - `date` → Date Format  
  - `time` → Time Format  
  - `price` → Decimal  
- **Handle Missing Values:** Replace nulls with default values or use `Remove Nulls`.
- **Extract Hour from Time:**  
  `Hour = HOUR(orders[time])`
- **Extract First Ingredient:**  
  `First Ingredient = LEFT(pizza_types[ingredients], FIND(",", pizza_types[ingredients]) - 1)`

</details>

---

## PART 2: Creating the Data Model

### 📌 Model Design
✅ Arrange tables properly in **Model View**  
✅ Define primary and foreign key relationships  
✅ Ensure correct **one-to-many** relationships  
✅ Check if any **inactive relationships** are needed  
✅ Set appropriate **filter flow directions**  

<details>
  <summary>💡 Show Solutions</summary>

- **Arrange Tables:** Place lookup tables (`pizza_types`, `pizzas`) above transaction tables (`order_details`, `orders`).
- **Relationships:**
  - `orders[order_id]` → `order_details[order_id]` (One-to-Many)
  - `pizzas[pizza_id]` → `order_details[pizza_id]` (One-to-Many)
  - `pizza_types[pizza_type_id]` → `pizzas[pizza_type_id]` (One-to-Many)
- **Cardinality:** One-to-Many (Orders → Order Details).
- **Inactive Relationships:** None required.
- **Filter Direction:** One-way filtering from lookup tables to transaction tables.

</details>

---

## PART 3: Adding DAX Measures

### 📌 Key Measures
Before revealing the formulas, try creating the following DAX measures:

✅ `Total Orders`  
✅ `Total Pizzas Sold`  
✅ `Total Revenue`  
✅ `Average Order Value`  
✅ `Most Popular Pizza Type`  
✅ `Least Ordered Pizza Type`  
✅ `Total Revenue by Category`  
✅ `Cumulative Revenue`  

<details>
  <summary>💡 Show DAX Measures</summary>

```DAX
Total Orders = COUNT(orders[order_id])
Total Pizzas Sold = SUM(order_details[quantity])
Total Revenue = SUMX(order_details, order_details[quantity] * pizzas[price])
Average Order Value = [Total Revenue] / [Total Orders]
Most Popular Pizza = TOPN(1, VALUES(order_details[pizza_id]), [Total Pizzas Sold])
Least Popular Pizza = TOPN(1, VALUES(order_details[pizza_id]), [Total Pizzas Sold], ASC)
Total Revenue by Category = SUMX(order_details, order_details[quantity] * pizzas[price])
Cumulative Revenue = CALCULATE([Total Revenue], FILTER(ALL(orders[date]), orders[date] <= MAX(orders[date])))
```

</details>

---

## PART 4: Building the Report

### 📊 **Dashboard Layout**

#### **🔹 KPI Section**
📌 **Total Revenue**  
📌 **Total Orders**  
📌 **Total Pizzas Sold**  
📌 **Average Order Value**  

#### **🔹 Visualizations**
📊 **Revenue by Pizza Category** → Bar Chart  
📊 **Most Popular & Least Popular Pizzas** → Column Chart  
📊 **Revenue Over Time** → Line Chart  
📊 **Orders by Region** → Map Visual  
📊 **Date Slicer** → Filter Orders by Time Period  

---

### 🚀 **Advanced Features**

#### **🔹 Conditional Formatting**
🎨 **Profit Margin** → White-to-Green Scale  
🎨 **Return Rate** → White-to-Red Scale  
🎨 **Data Bars for Order Quantity**  

#### **🔹 Drillthrough Filters & Tooltips**
📌 Clicking on a pizza **filters** its sales trends.  
📌 Custom Tooltips show **Price, Category, and Sales Trends**.  

---

### ✅ **Final Steps**
🎯 Implement these features in Power BI.  
🎯 Optimize visual interactions.  
🎯 Capture a **Final Report Screenshot**.  

---
