📘 **Technical Details**

🔄 **Data Preparation & Transformation:**
Data Modeling

The dashboard uses a Star Schema Model:

- Orders → Fact Table
- Customers → Dimension Table
- Location → Dimension Table
- Product → Dimension Table

**Relationships:**

Table	Key:
- Orders ↔ Customers:	Customer ID
- Orders ↔ Product:	Product ID
- Orders ↔ Location:	Postal Code

⚙️ **Parameters:**

- Select Year Parameter: 
```
[Select Year]
```
- Used for dynamic year-based dashboard filtering.

📌 **Calculated Fields:**

**1. Date Calculations:**
- Current Year: 
```
[Select Year]
```
- Previous Year: 
```
[Select Year] - 1
```
- Order Date (Year): 
```
YEAR([Order Date])
```

**2. Sales KPI Calculations:**
- CY Sales:
```
IF YEAR([Order Date]) = [Select Year]
THEN [Sales]
END
```

- PY Sales:
```
IF YEAR([Order Date]) = [Select Year]-1
THEN [Sales]
END
```
- % Difference Sales:
```
(SUM([CY Sales]) - SUM([PY Sales])) / SUM([PY Sales])
```

- Min/Max Sales:
```
IF SUM([CY Sales]) = WINDOW_MAX(SUM([CY Sales]))
THEN SUM([CY Sales])
ELSEIF SUM([CY Sales]) = WINDOW_MIN(SUM([CY Sales]))
THEN SUM([CY Sales])
END
```

**3. Profit KPI Calculations:**

- CY Profit:
```
IF YEAR([Order Date]) = [Select Year]
THEN [Profit]
END
```

- PY Profit:
```
IF YEAR([Order Date]) = [Select Year]-1
THEN [Profit]
END
```

- % Difference Profit:
```
(SUM([CY Profit]) - SUM([PY Profit])) / SUM([CY Profit])
```

- Min/Max Profit:
```
IF SUM([CY Profit]) = WINDOW_MAX(SUM([CY Profit]))
THEN SUM([CY Profit])
ELSEIF SUM([CY Profit]) = WINDOW_MIN(SUM([CY Profit]))
THEN SUM([CY Profit])
END
```

**4. Quantity KPI Calculations:**

- CY Quantity:
```
IF YEAR([Order Date]) = [Select Year]
THEN [Quantity]
END
```

- PY Quantity:
```
IF YEAR([Order Date]) = [Select Year]-1
THEN [Quantity]
END
```

- % Difference Quantity:
```
(SUM([CY Quantity]) - SUM([PY Quantity])) / SUM([PY Quantity])
```

- Min/Max Quantity:
```
IF SUM([CY Quantity]) = WINDOW_MAX(SUM([CY Quantity]))
THEN SUM([CY Quantity])
ELSEIF SUM([CY Quantity]) = WINDOW_MIN(SUM([CY Quantity]))
THEN SUM([CY Quantity])
END
```

**5. Subcategory Comparison:**

- KPI CY Less PY:
```
IF SUM([CY Sales]) < SUM([PY Sales])
THEN '⬤'
ELSE ''
END
```

**6. Weekly Trends:**

- KPI Sales Avg:
```
IF SUM([CY Sales]) > WINDOW_AVG(SUM([CY Sales]))
THEN 'Above'
ELSE 'Below'
END
```

- KPI Profit Avg:
```
IF SUM([CY Profit]) > WINDOW_AVG(SUM([CY Profit]))
THEN 'Above'
ELSE 'Below'
END
```

**7. Customer KPI Calculations:**

- CY Customers:
```
IF YEAR([Order Date]) = [Select Year]
THEN [Customer ID]
END
```

- PY Customers:
```
IF YEAR([Order Date]) = [Select Year]-1
THEN [Customer ID]
END
```

- % Difference Customers:
```
(COUNTD([CY Customers]) - COUNTD([PY Customers])) / COUNTD([PY Customers])
```

- Min/Max Customers:
```
IF COUNTD([CY Customers]) = WINDOW_MAX(COUNTD([CY Customers]))
THEN COUNTD([CY Customers])
ELSEIF COUNTD([CY Customers]) = WINDOW_MIN(COUNTD([CY Customers]))
THEN COUNTD([CY Customers])
END
```

**8. Sales Per Customer:**

- CY Sales per Customer:
```
SUM([CY Sales]) / COUNTD([CY Customers])
```

- PY Sales per Customer:
```
SUM([PY Sales]) / COUNTD([PY Customers])
```

- % Difference Sales per Customer:
```
([CY Sales per Customer] - [PY Sales per Customer]) / [PY Sales per Customer]
```

**9. Orders KPI:**

- CY Orders:
```
IF YEAR([Order Date]) = [Select Year]
THEN [Order ID]
END
```

- PY Orders:
```
IF YEAR([Order Date]) = [Select Year]-1
THEN [Order ID]
END
```

- % Difference Orders:
```
(COUNTD([CY Orders]) - COUNTD([PY Orders])) / COUNTD([PY Orders])
```

**10. Customer Distribution:**

- No. of Orders per Customer:
```
{FIXED [CY Customers]: COUNTD([CY Orders])}
```

📈 **Dashboard Architecture:**
- Pages Included:
    - Sales Dashboard: Displays sales, profit, quantity, weekly trends, and product subcategory analysis.
    - Customer Dashboard: Displays customer KPIs, customer distribution, sales per customer, and top customers analysis.
    - Filter Panel: Provides dynamic filters for Year, Category, Sub-Category, Region, State, and City.

- Navigation Buttons:

  The dashboard includes interactive navigation buttons for smooth page transition:

    - Sales Button → Navigates to Sales Dashboard
    - Customer Button → Navigates to Customer Dashboard
    - Filter Button → Opens Filter Panel

- Interactive Features:
    - Dynamic Year Selection
    - Product & Location Filters
    - Interactive Charts
    - Cross-filtering between visualizations
    - Hover Tooltips for detailed insights

📌 **Technical Highlights:**
- Implemented Year-over-Year analysis for sales and customer comparison.
- Designed a star schema data model using fact and dimension tables.
- Created dynamic KPIs and calculated fields for business analysis.
- Added interactive filters and navigation buttons for better usability.
- Used conditional formatting to highlight performance trends.
- Performed subcategory and customer profitability analysis for insights.
