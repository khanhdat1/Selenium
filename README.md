# BÁO CÁO BÀI TẬP: KIỂM THỬ TỰ ĐỘNG VỚI SELENIUM

## 1. Thông Tin Sinh Viên

Họ và tên: Nguyễn Nam Khánh
Mã sinh viên: 23010771
Môn học: Đảm bảo chất lượng và Kiểm thử phần mềm

## 2. Kịch Bản Kiểm Thử (Test Cases)

Bài tập thực hiện kiểm thử tự động trên website: https://www.saucedemo.com/

| Mã TC | Tên Chức Năng              | Các Bước Thực Hiện                                                                                                                                                                           | Kết Quả Mong Đợi                                                   | Trạng Thái |
| ----- | -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | ---------- |
| TC01  | Đăng nhập thành công       | 1. Vào trang Login. <br> 2. Nhập username `standard_user` và password `secret_sauce`. <br> 3. Click nút "Login".                                                                             | Đăng nhập thành công, hệ thống chuyển sang trang Products.         | PASSED     |
| TC02  | Thêm sản phẩm vào giỏ hàng | 1. Đăng nhập vào hệ thống. <br> 2. Click nút "Add to cart" của sản phẩm Sauce Labs Backpack. <br> 3. Kiểm tra số lượng hiển thị trên giỏ hàng. <br> 4. Mở giỏ hàng và kiểm tra tên sản phẩm. | Giỏ hàng hiển thị số lượng 1 và có sản phẩm "Sauce Labs Backpack". | PASSED     |
| TC03  | Đăng xuất hệ thống         | 1. Đăng nhập vào hệ thống. <br> 2. Click nút menu ở góc trái. <br> 3. Click nút "Logout".                                                                                                    | Đăng xuất thành công, hệ thống quay về trang Login.                | PASSED     |

## 3. Công Cụ Sử Dụng

Ngôn ngữ: Python 3.13
Thư viện: selenium, pytest
Trình duyệt điều khiển: Google Chrome
Công cụ soạn thảo: Visual Studio Code

## 4. Hướng Dẫn Chạy Mã Nguồn

Clone repository này về máy.

Cài đặt thư viện:

```bash
pip install selenium pytest
```

Chạy file test:

```bash
python Lab9.py
```

Hoặc có thể chạy bằng lệnh:

```bash
pytest -v Lab9.py
```

## 5. Mã Nguồn Kiểm Thử

```python
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC


BASE_URL = "https://www.saucedemo.com/"
USERNAME = "standard_user"
PASSWORD = "secret_sauce"


@pytest.fixture
def driver():
    driver = webdriver.Chrome()
    driver.maximize_window()
    yield driver
    driver.quit()


def login(driver):
    driver.get(BASE_URL)
    driver.find_element(By.ID, "user-name").send_keys(USERNAME)
    driver.find_element(By.ID, "password").send_keys(PASSWORD)
    driver.find_element(By.ID, "login-button").click()


# TC01: Kiểm thử đăng nhập thành công
def test_login_success(driver):
    login(driver)

    WebDriverWait(driver, 10).until(
        EC.visibility_of_element_located((By.CLASS_NAME, "inventory_list"))
    )

    assert "inventory.html" in driver.current_url
    assert driver.find_element(By.CLASS_NAME, "title").text == "Products"


# TC02: Kiểm thử thêm sản phẩm vào giỏ hàng
def test_add_product_to_cart(driver):
    login(driver)

    WebDriverWait(driver, 10).until(
        EC.element_to_be_clickable((By.ID, "add-to-cart-sauce-labs-backpack"))
    ).click()

    cart_badge = driver.find_element(By.CLASS_NAME, "shopping_cart_badge")
    assert cart_badge.text == "1"

    driver.find_element(By.CLASS_NAME, "shopping_cart_link").click()

    product_name = WebDriverWait(driver, 10).until(
        EC.visibility_of_element_located((By.CLASS_NAME, "inventory_item_name"))
    )

    assert product_name.text == "Sauce Labs Backpack"


# TC03: Kiểm thử đăng xuất
def test_logout(driver):
    login(driver)

    WebDriverWait(driver, 10).until(
        EC.element_to_be_clickable((By.ID, "react-burger-menu-btn"))
    ).click()

    WebDriverWait(driver, 10).until(
        EC.element_to_be_clickable((By.ID, "logout_sidebar_link"))
    ).click()

    WebDriverWait(driver, 10).until(
        EC.visibility_of_element_located((By.ID, "login-button"))
    )

    assert driver.current_url == BASE_URL


if __name__ == "__main__":
    pytest.main([__file__, "-v"])
```

## 6. Kết Quả Thực Hiện

Sau khi chạy chương trình, Selenium tự động mở trình duyệt Google Chrome và thực hiện lần lượt 03 test case:

* TC01: Đăng nhập thành công.
* TC02: Thêm sản phẩm vào giỏ hàng.
* TC03: Đăng xuất hệ thống.

Kết quả chạy kiểm thử hiển thị cả 03 test case đều ở trạng thái PASSED.

## 7. Kết Luận

Thông qua bài thực hành này, sinh viên đã hiểu cách sử dụng Selenium WebDriver để tự động hóa thao tác trên trình duyệt web. Bài kiểm thử đã áp dụng Selenium để kiểm tra các chức năng cơ bản của website thương mại điện tử như đăng nhập, thêm sản phẩm vào giỏ hàng và đăng xuất. Đây là nền tảng quan trọng để sinh viên có thể tiếp tục phát triển các kịch bản kiểm thử tự động phức tạp hơn trong thực tế.
