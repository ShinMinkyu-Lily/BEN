import unittest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

class RegistrationTest(unittest.TestCase):
    @classmethod
    def setUpClass(cls):
        cls.driver = webdriver.Chrome()
        cls.driver.implicitly_wait(5)
        cls.base_url = "http://18.216.201.86/"

    def fill_form(self, username, password, email, subscribe_newsletter=False):
        driver = self.driver
        driver.get(self.base_url)
        driver.find_element(By.NAME, "username").send_keys(username)
        driver.find_element(By.NAME, "password").send_keys(password)
        driver.find_element(By.NAME, "email").send_keys(email)
        newsletter = driver.find_element(By.NAME, "subscribe")
        if subscribe_newsletter != newsletter.is_selected():
            newsletter.click()
        driver.find_element(By.CSS_SELECTOR, "button[type='submit']").click()

    def test_valid_registration_with_newsletter(self):
        self.fill_form("seleniumUser", "StrongP@ss1", "test@benic.ai", True)
        success = WebDriverWait(self.driver, 10).until(
            EC.visibility_of_element_located((By.ID, "successMessage"))
        )
        self.assertIn("Registration successful", success.text)

    def test_invalid_email_format(self):
        self.fill_form("user2", "AnotherP@ss2", "bad-email-format")
        error = self.driver.find_element(By.ID, "emailError")
        self.assertEqual(error.text, "Enter a valid email address")

    @classmethod
    def tearDownClass(cls):
        cls.driver.quit()

if __name__ == "__main__":
    unittest.main()
