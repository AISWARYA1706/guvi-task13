# guvi-task13

from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.by import By
import pytest
import time
@pytest.fixture
def setup():
    # Setup Chrome driver
    driver = webdriver.Chrome()
    driver.maximize_window()
    driver.get("https://jqueryui.com/droppable/")
    yield driver
    driver.quit()


def test_drag_and_drop_positive(setup):
    driver = setup

    # Switch to iframe that contains the draggable box
    driver.switch_to.frame(driver.find_element(By.CLASS_NAME, "demo-frame"))

    # Locate elements
    source = driver.find_element(By.ID, "draggable")
    target = driver.find_element(By.ID, "droppable")

    # Perform drag and drop
    actions = ActionChains(driver)
    actions.drag_and_drop(source, target).perform()
    time.sleep(2)

    # Verify success text after dropping
    assert "Dropped!" in target.text, "Drag and Drop failed!"

    print("Positive Test Passed: Drag and Drop successful.")


def test_drag_and_drop_negative(setup):
    driver = setup

    # Switch to iframe
    driver.switch_to.frame(driver.find_element(By.CLASS_NAME, "demo-frame"))

    # Locate elements
    source = driver.find_element(By.ID, "draggable")
    target = driver.find_element(By.ID, "droppable")

    # Do not perform drag-and-drop (negative test)
    time.sleep(2)

    # Verify it has NOT been dropped
    assert "Drop here" in target.text, " Box moved unexpectedly!"

    print(" Negative Test Passed: Box did not move without drag-and-drop.")
