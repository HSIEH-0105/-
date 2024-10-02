from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from webdriver_manager.chrome import ChromeDriverManager

def search_fraud_website(target_url):
    # 配置 Chrome 瀏覽器
    chrome_options = Options()
    chrome_options.add_argument("--headless")  # 啟用無頭模式
    chrome_options.add_argument("--disable-gpu")
    
    # 啟動 Selenium WebDriver
    driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=chrome_options)
    
    # 進入 165 反詐騙網站
    driver.get("https://165.npa.gov.tw/#/")
    
    # 模擬輸入查詢網址（根據網站的查詢欄設置）
    search_input = driver.find_element(By.ID, 'input-query')  # 這需要根據實際的網站查詢欄位ID
    search_input.send_keys(target_url)
    
    # 模擬按下查詢按鈕
    search_button = driver.find_element(By.ID, 'submit-search')  # 這需要根據實際的按鈕ID
    search_button.click()
    
    # 等待頁面加載並獲取結果
    driver.implicitly_wait(10)
    
    # 獲取查詢結果的元素，並返回結果文本
    try:
        result = driver.find_element(By.CLASS_NAME, 'result-class').text  # 替換成實際的結果區域 class 名稱
    except:
        result = "查無結果或無法查詢"
    
    # 關閉瀏覽器
    driver.quit()
    
    return result

# 測試爬取某個網址的詐騙查詢結果
url_to_check = "http://example.com"
result = search_fraud_website(url_to_check)
print(result)
