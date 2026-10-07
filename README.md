# alertexercise
### Problem Statement

You can use a demo shopping website such as SauceDemo (Swag Labs) for login, product, cart, and checkout exercises. For alert, mouse, drag-and-drop, and dynamic-element exercises, a dedicated Selenium demo site is more suitable because SauceDemo does not provide all those interactions.

| Test Case | Shopping Scenario | Selenium Concept | Expected Result |
|---|---|---|---|
| TC01 | Open the online shopping website | `driver.get()` | Shopping website opens successfully |
| TC02 | Customer clicks Delete/Remove Product and confirmation popup appears | Alert – `accept()` | Product deletion is confirmed |
| TC03 | Customer clicks Delete/Remove Product but chooses Cancel | Alert – `dismiss()` | Product remains in the cart |
| TC04 | Customer enters a name/coupon/customer information in a prompt popup | Prompt – `send_keys()` | Entered information is submitted successfully |
| TC05 | Customer moves the mouse over the Products/Category menu | Mouse Hover | Product categories/submenu are displayed |
| TC06 | Customer double-clicks a product | Double Click | Product details page opens |
| TC07 | Customer drags a product/item into a shopping cart area | Drag & Drop | Product is moved to the cart |
| TC08 | Customer searches for a product and waits for the product results to load | Explicit Wait | Product is displayed successfully |
| TC09 | Customer completes checkout and waits until the Place Order button becomes clickable | Clickable Wait | Order is submitted successfully |
| TC10 | Customer completes the purchase and waits for the order confirmation popup | Alert Wait | Confirmation alert is handled successfully |
### Code
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time


driver = webdriver.Chrome()
driver.maximize_window()

wait = WebDriverWait(driver, 10)

#### TC01 - OPEN ONLINE SHOPPING WEBSITE

print("\nTC01 - Open Shopping Website")

driver.get("https://www.saucedemo.com/")

wait.until(
    EC.presence_of_element_located(
        (By.ID, "user-name")
    )
)

print("Shopping website opened successfully")

driver.find_element(
    By.ID,
    "user-name"
).send_keys("standard_user")

driver.find_element(
    By.ID,
    "password"
).send_keys("secret_sauce")

driver.find_element(
    By.ID,
    "login-button"
).click()

wait.until(
    EC.url_contains("inventory.html")
)

print("Login successful")

#### TC02 - DELETE / REMOVE PRODUCT

print("\nTC02 - Delete / Remove Product")

add_product = wait.until(
    EC.element_to_be_clickable(
        (By.ID, "add-to-cart-sauce-labs-backpack")
    )
)

add_product.click()

print("Product added to cart")

driver.find_element(
    By.CLASS_NAME,
    "shopping_cart_link"
).click()

wait.until(
    EC.presence_of_element_located(
        (By.CLASS_NAME, "cart_item")
    )
)

remove_product = wait.until(
    EC.element_to_be_clickable(
        (By.ID, "remove-sauce-labs-backpack")
    )
)

remove_product.click()

print("Product removed from cart")

#### TC03 - CONFIRMATION ALERT - CANCEL

print("\nTC03 - Confirmation Alert - Cancel")

driver.get(
    "https://the-internet.herokuapp.com/javascript_alerts"
)

wait.until(
    EC.presence_of_element_located(
        (
            By.CSS_SELECTOR,
            "button[onclick*='jsConfirm']"
        )
    )
)

driver.find_element(
    By.CSS_SELECTOR,
    "button[onclick*='jsConfirm']"
).click()

alert = wait.until(
    EC.alert_is_present()
)

print("Confirmation message:")
print(alert.text)

alert.dismiss()

print("Confirmation cancelled successfully")

#### TC04 - PROMPT ALERT

print("\nTC04 - Prompt Alert")

driver.find_element(
    By.CSS_SELECTOR,
    "button[onclick*='jsPrompt']"
).click()

alert = wait.until(
    EC.alert_is_present()
)

print("Prompt message:")
print(alert.text)

alert.send_keys("Shehan")

print("Entered information: Shehan")

alert.accept()

print("Prompt submitted successfully")

#### TC05 - MOUSE HOVER

print("\nTC05 - Mouse Hover")

driver.get(
    "https://the-internet.herokuapp.com/hovers"
)

wait.until(
    EC.presence_of_element_located(
        (By.CLASS_NAME, "figure")
    )
)

figures = driver.find_elements(
    By.CLASS_NAME,
    "figure"
)

first_figure = figures[0]

ActionChains(driver).move_to_element(
    first_figure
).perform()

print("Mouse hover performed successfully")

time.sleep(2)

#### TC06 - DOUBLE CLICK

print("\nTC06 - Double Click")

driver.get("https://demoqa.com/buttons")

double_click_button = wait.until(
    EC.element_to_be_clickable(
        (By.ID, "doubleClickBtn")
    )
)

ActionChains(driver).double_click(
    double_click_button
).perform()

print("Double click performed successfully")

time.sleep(2)

#### TC07 - DRAG AND DROP

print("\nTC07 - Drag and Drop")

driver.get(
    "https://the-internet.herokuapp.com/drag_and_drop"
)

source = wait.until(
    EC.presence_of_element_located(
        (By.ID, "column-a")
    )
)

target = wait.until(
    EC.presence_of_element_located(
        (By.ID, "column-b")
    )
)

ActionChains(driver).drag_and_drop(
    source,
    target
).perform()

print("Drag and drop performed successfully")

time.sleep(2)


#### TC08 - EXPLICIT WAIT

print("\nTC08 - Explicit Wait")

driver.get(
    "https://www.saucedemo.com/"
)

wait.until(
    EC.visibility_of_element_located(
        (By.ID, "user-name")
    )
)

driver.find_element(
    By.ID,
    "user-name"
).send_keys("standard_user")

driver.find_element(
    By.ID,
    "password"
).send_keys("secret_sauce")

driver.find_element(
    By.ID,
    "login-button"
).click()

wait.until(
    EC.presence_of_element_located(
        (By.CLASS_NAME, "inventory_item")
    )
)

print("Product results loaded successfully")

#### TC09 - CLICKABLE WAIT

print("\nTC09 - Clickable Wait")

driver.get("https://www.saucedemo.com/")

wait.until(
    EC.visibility_of_element_located(
        (By.ID, "user-name")
    )
).send_keys("standard_user")

driver.find_element(
    By.ID, "password"
).send_keys("secret_sauce")

driver.find_element(
    By.ID, "login-button"
).click()

wait.until(
    EC.visibility_of_element_located(
        (By.CLASS_NAME, "inventory_item")
    )
)

add_to_cart = wait.until(
    EC.element_to_be_clickable(
        (By.ID, "add-to-cart-sauce-labs-backpack")
    )
)

add_to_cart.click()

print("Product added to cart")


wait.until(
    EC.element_to_be_clickable(
        (By.CLASS_NAME, "shopping_cart_link")
    )
).click()


wait.until(
    EC.visibility_of_element_located(
        (By.CLASS_NAME, "cart_item")
    )
)

wait.until(
    EC.element_to_be_clickable(
        (By.ID, "checkout")
    )
).click()

wait.until(
    EC.visibility_of_element_located(
        (By.ID, "first-name")
    )
).send_keys("Shehan")

driver.find_element(
    By.ID, "last-name"
).send_keys("Shajahan")

driver.find_element(
    By.ID, "postal-code"
).send_keys("600001")

driver.find_element(
    By.ID, "continue"
).click()


finish = wait.until(
    EC.element_to_be_clickable(
        (By.ID, "finish")
    )
)

finish.click()

print("Order submitted successfully")

time.sleep(2)

#### TC10 - ORDER CONFIRMATION

print("\nTC10 - Order Confirmation")

confirmation = wait.until(
    EC.visibility_of_element_located(
        (By.CLASS_NAME, "complete-header")
    )
)

print("Confirmation message:")
print(confirmation.text)

print("Order confirmation displayed successfully")

print("ALL TEST CASES COMPLETED SUCCESSFULLY")

time.sleep(3)

driver.quit()

print("Browser closed")

input("Press Enter to exit...")
```

### Output
<img width="1917" height="967" alt="image" src="https://github.com/user-attachments/assets/4ffbf21a-35fa-402c-80bb-3fba17c3f5a7" />
<img width="1912" height="735" alt="image" src="https://github.com/user-attachments/assets/83b8b228-48b2-442b-8715-68f0f338f6a5" />
<img width="1917" height="988" alt="image" src="https://github.com/user-attachments/assets/5a9398c3-7581-4681-8bbf-0e6561b209cf" />
<img width="1916" height="1002" alt="image" src="https://github.com/user-attachments/assets/bb0cf485-704b-4738-afcd-b2fbf5263fa3" />
<img width="1915" height="982" alt="image" src="https://github.com/user-attachments/assets/7c9e2a19-e9be-4394-aeec-fa811954ea3e" />
<img width="1413" height="953" alt="image" src="https://github.com/user-attachments/assets/49f79d6e-f903-4ba3-be98-5f9bb5221fb7" />
<img width="1917" height="952" alt="image" src="https://github.com/user-attachments/assets/12bb00bb-0e4d-496a-87a6-3af1b546415b" />
<img width="1913" height="917" alt="image" src="https://github.com/user-attachments/assets/52f6da69-da44-4dea-a822-4646feebe823" />
