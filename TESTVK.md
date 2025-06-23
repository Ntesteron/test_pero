import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from webdriver_manager.chrome import ChromeDriverManager

@pytest.fixture(scope="function")
def driver():
    driver = webdriver.Chrome(ChromeDriverManager().install())
    yield driver
    driver.quit()

def test_product_card(driver):

    driver.get("https://www.wildberries.ru/catalog/177088689/detail.aspx")
    
    WebDriverWait(driver, 10).until(
        EC.visibility_of_element_located((By.CSS_SELECTOR, ".product-page__header"))
    )
    
   
    title = driver.find_element(By.CSS_SELECTOR, ".product-page__header h1").text
    assert title, "Название товара не отображается"
    
    price = driver.find_element(By.CSS_SELECTOR, ".price-block__final-price").text
    assert price, "Цена товара не отображается"
    
    buy_button = driver.find_element(By.CSS_SELECTOR, ".btn-main")
    assert buy_button.is_displayed(), "Кнопка 'Купить' не отображается"
    buy_button.click()
    
    WebDriverWait(driver, 5).until(
        EC.visibility_of_element_located((By.CSS_SELECTOR, ".j-basket-notification"))
    )
    ________________________________________
    TEST2
    def test_community_page(driver):
    driver.get("https://vk.com/pero_social")
    
    WebDriverWait(driver, 10).until(
        EC.visibility_of_element_located((By.CSS_SELECTOR, ".page_name"))
    )
    
    title = driver.find_element(By.CSS_SELECTOR, ".page_name").text
    assert "PERO" in title, "Название сообщества не совпадает"
    
    posts = driver.find_elements(By.CSS_SELECTOR, ".post")
    assert len(posts) > 0, "Посты не найдены"
  
    subscribe_button = driver.find_element(By.CSS_SELECTOR, ".page_actions .flat_button")
    assert subscribe_button.is_displayed(), "Кнопка 'Подписаться' не отображается"
    subscribe_button.click()
    
    WebDriverWait(driver, 5).until(
        EC.text_to_be_present_in_element((By.CSS_SELECTOR, ".page_actions .flat_button"), "Отписаться")
    )
