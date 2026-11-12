# Brew & Byte Café Ordering System

A command-line point-of-sale (POS) and order management system written in Python for **ITT212 Python Programming**. It supports menu management, order customization, tiered discounts, GCT calculation, customer loyalty points, and manager sales reporting.

---

## Prerequisites & Installation

* **Python Version:** Python 3.6 or higher
* **Dependencies:** Standard library only (`os`, `datetime`). No external packages required.

### How to Run
1. Save the source code as `brew_and_byte.py` (or your preferred filename).
2. Open a terminal or command prompt and navigate to the folder containing the file.
3. Run the program:
   ```bash
   python brew_and_byte.py
   ```
   *(Note: On macOS/Linux, you may need to use `python3 brew_and_byte.py`)*

---

## Data Files

The system automatically manages three flat text files in the same directory as the script:

| File | Purpose | Format | Fallback / Auto-Creation |
| :--- | :--- | :--- | :--- |
| `menu.txt` | Stores custom café menu items. | `Code,Name,Category,Price` | Falls back to a built-in 10-item Jamaican café menu if missing. |
| `loyalty.txt` | Stores registered loyalty members and point balances. | `MemberID,Full Name,Points` | Created automatically when the first member is registered. |
| `orders.txt` | Logs all completed checkout transactions. | `Timestamp|Items|Subtotal|Discount|Tax|Total` | Created automatically upon the first completed checkout. |

---

## Interface Walkthrough & Instructions

### 1. Main Menu
When you launch the program, the main dashboard displays your live item count, current subtotal, and 9 navigation options.

```text
================================================
                BREW & BYTE CAFÉ                
             Campus Ordering System             
================================================
Current order: 0 item(s)     Subtotal: $0.00 JMD
------------------------------------------------
1. View menu
2. Add item to order
3. Remove item from order
4. View current order
5. Update item quantity
6. Checkout
7. Cancel order
8. Manager options
0. Exit
------------------------------------------------
Enter your choice (0-8): 
```

* **Instructions:** Enter a whole number between `0` and `8` and press **Enter**. Invalid letters or out-of-range numbers will prompt you to try again.

---

### 2. Viewing & Building an Order (Options 1 – 5 & 7)

#### Option 1 & 2: View Menu & Add Item
Selecting **1** displays the café menu grouped by category. Selecting **2** displays the menu and prompts you to add items.

```text
=================================================================
                      BREW & BYTE CAFÉ MENU                      
=================================================================

--- HOT DRINKS ---
Code     Item                                          Price
-----------------------------------------------------------------
C01      Regular Coffee                          $350.00 JMD
C02      Cappuccino                              $550.00 JMD
C03      Hot Chocolate                           $450.00 JMD

--- COLD DRINKS ---
Code     Item                                          Price
-----------------------------------------------------------------
D01      Iced Latte                              $600.00 JMD
D02      Fruit Smoothie                          $750.00 JMD
D03      Bottled Water                           $150.00 JMD

--- FOOD ---
Code     Item                                          Price
-----------------------------------------------------------------
F01      Beef Patty                              $400.00 JMD
F02      Chicken Sandwich                        $850.00 JMD
F03      Banana Muffin                           $300.00 JMD
F04      Ackee and Saltfish Wrap                 $950.00 JMD
=================================================================
Enter item code: F01
Enter quantity (1-20): 2
Added 2 x Beef Patty to the order.
```

* **Instructions:**
  1. Enter a valid item code (case-insensitive, e.g., `f01` or `F01`).
  2. Enter a quantity between `1` and `20`. Adding an existing item increases its quantity.

#### Option 3, 4, 5 & 7: Manage Current Order
* **Option 4 (View current order):** Displays an itemized breakdown of your cart:
  ```text
  ========================================================================
                               CURRENT ORDER                              
  ========================================================================
  Code   Item                         Qty       Unit Price       Line Total
  ------------------------------------------------------------------------
  F01    Beef Patty                     2      $400.00 JMD      $800.00 JMD
  C02    Cappuccino                     1      $550.00 JMD      $550.00 JMD
  ------------------------------------------------------------------------
  Subtotal:                                                   $1,350.00 JMD
  ========================================================================
  ```
* **Option 3 (Remove item):** Shows the current order and asks for the item code to delete completely.
* **Option 5 (Update quantity):** Shows the current order, asks for the item code, and replaces its quantity with a new number (`1–20`).
* **Option 7 (Cancel order):** Asks for `y/n` confirmation before clearing all items from your cart.

---

### 3. Checkout, Discounts & Loyalty (Option 6)

Selecting **6** walks through customer classification, loyalty redemption, and receipt generation.

#### Discount & Tax Rules
* **Student ID:** `10%` off subtotal.
* **Staff ID:** `5%` off subtotal.
* **Bulk Discount:** Additional `$200.00 JMD` off if the subtotal is `$2,500.00 JMD` or more.
* **GCT Tax:** `15%` applied to the subtotal *after* discounts.
* **Loyalty Rewards:** Earn `1 point` per `$100.00 JMD` spent (rounded down). Redeem `50 points` for a free **Regular Coffee**.

#### Checkout Flow & Receipt
```text
Customer Type:
1. Regular (No ID discount)
2. Student (10% discount with valid ID)
3. Staff (5% discount with valid ID)
Enter your choice (1-3): 2
Is the customer a loyalty member? (y/n): y
Enter Member ID (e.g., M001): M001
Welcome back, Jane Doe! Current points: 65
You have 50+ points! Redeem 50 points for a free Regular Coffee? (y/n): y
50 points redeemed! Free Regular Coffee added.

==============================================
               BREW & BYTE CAFÉ               
==============================================
Beef Patty               x2   $     800.00 JMD
Cappuccino               x1   $     550.00 JMD
Regular Coffee (Reward)  x1   $       0.00 JMD
----------------------------------------------
Subtotal                     $   1,350.00 JMD
Discount (student)          -$     135.00 JMD
GCT (15%)                    $     182.25 JMD
TOTAL                        $   1,397.25 JMD
==============================================
   Thank you for visiting Brew & Byte Café!   
==============================================
Loyalty points earned: 13 | New balance: 28 pts
```

* **Instructions:**
  1. Select `1`, `2`, or `3` to apply applicable ID discounts.
  2. Answer `y` or `n` for loyalty membership. If `y`, enter a registered ID (e.g., `M001`).
  3. If the member has 50+ points, choose whether to redeem a free coffee.
  4. The receipt prints automatically, points are updated in `loyalty.txt`, and the order is archived in `orders.txt`.

---

### 4. Manager Options (Option 8)

Selecting **8** requires administrative authentication before opening the manager dashboard.

* **Default Manager PIN:** `1234`

```text
Enter manager PIN: 1234

---------- MANAGER OPTIONS ----------
1. View daily sales report
2. Register loyalty member
3. View loyalty members
0. Return to main menu
-------------------------------------
Enter your choice (0-3): 
```

#### Manager Sub-Menu Features
* **Option 1 (View daily sales report):** Reads `orders.txt` and calculates total orders, gross revenue, total GCT collected, and the top-selling item:
  ```text
  ================================================
             DAILY SALES SUMMARY REPORT           
  ================================================
  Total Orders:             4
  Total Revenue:            $6,420.50 JMD
  Total GCT Collected:      $837.46 JMD
  Best-Selling Item:        Beef Patty (6 sold)
  ================================================
  ```
* **Option 2 (Register loyalty member):** Prompts for a full name, auto-generates the next sequential ID (`M001`, `M002`, etc.) with `0` starting points, and saves it to `loyalty.txt`.
* **Option 3 (View loyalty members):** Displays a formatted table of all registered members and their current point balances.
* **Option 0 (Return to main menu):** Exits manager mode back to the main ordering screen.

