
# Food Delivery Analysis

## Bangalore outlets from the food delivery platform Zomato 
URL =https://www.zomato.com/bangalore

![image](https://github.com/user-attachments/assets/bc47534f-2f53-48e0-a920-5683fecad51b)


This project focuses on extracting, cleaning, and analyzing data from the for Bangalore outlets from the food delivery platform Zomato .
By utilizing web scraping techniques, we aim to gather detailed information about various Restaurants available on the platform. 
The project includes several key stages: web scraping, data cleaning, data analysis  and visualization. 
Additionally, insights are presented through a PowerPoint presentation and an interactive dashboard.


## Collaborators

- [Kunal Jain](https://github.com/hrugwedm)
- [Gayatri Tumsare](https://github.com/gayu47)
- [Hrugwed More](https://github.com/10kunalJain)
- [Saswat Nayak](https://github.com/Saswat132002)
- [Afreen Ansar](https://github.com/05afreen)

## Key Objectives:
- Web Scraping: Automate the extraction of relevant data from the zomato website.
- Data Cleaning: Ensure the data is accurate and consistent for analysis.
- Visualization: Create an interactive dashboard to visualize the data and highlight key insights.
- Presentation: Summarize the findings in a PowerPoint presentation for clear communication of results.

## Project Deliverables:
- Web Scraping Code: Python scripts using Selenium to scrape data from the zomato website.
- Cleaned Data: A cleaned dataset saved in CSV format.
- PowerPoint Presentation: Slides summarizing the project, data insights, and key findings.
- Interactive Dashboard: A dashboard for visualizing data, highlighting price distribution, top restaurant, average ratings, locations and cuisine analysis.

## Table of Contents 
    1. Introduction
    2. Ethical Scraping Practices
    3. Web Scraping Implementation
    4. Data Cleaning
    5. Data Analysis
    6. Dashboard Creation
    7. Conclusion

### 1. Introduction
This project involves scraping data from the Zomato website using Python and Selenium. The aim is to collect detailed information about various restaurants and then perform data analysis to extract meaningful insights. The insights will be presented in a PowerPoint presentation, and an interactive dashboard will be created for end-users to explore the data.
    
### 2. Ethical Scraping Practices
- Adhered to ethical scraping practices and respected the website's terms of service.
- Implemented delays between requests to avoid overloading the website's servers.
- Used polite scraping techniques to minimize the load on the website's resources.

### 3. Web Scraping Implementation

#### Deployment

To run this project first you need these libraries and follow the steps mentioned

#### Libraries Nedded
- pandas for data manipulation and analysis
- numpy for numerical operations
- selenium for web scraping
- time for giving delays while scrapping
- re for regular expression operations

#### Step 1: Import libraries, initialize the WebDriver and Open the Website
```bash#import all the required Libraries 
import numpy as np
import os
import pandas as pd
import time
import re
import requests
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException, NoSuchElementException
driver=webdriver.Chrome()  # open the chrome 
driver.maximize_window()   # maximize the window
driver.get("https://www.zomato.com/bangalore")  #to the web page
```

#### Step 2: Scrape Data from Pages

```bash
driver.get("https://www.zomato.com/bangalore")  #to the web page
```

#### Step 3: Define Dictionaries for Scraping Data

```bash# dictionary  of required column
data = {"name":[],"price":[],"cuisine":[],"location":[],"link":[],"rating":[]}
           
second_page={"name":[] ,"Timings":[],"Resturant_known_for":[], "delivery_review_number": [], "dish_name": []} 

```

#### Step 4: Extract Values Using Defined Dictionary
```bash#Looping over to get the data and append them in the dictionary 
for i in range(5):
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")  # get the screen height of the web
    time.sleep(5)
    content = driver.find_elements(By.XPATH,"//div[@class='jumbo-tracker']") # parent class of each element 

    for j in content:
        try:
            data["name"].append(j.find_element(By.CLASS_NAME, "sc-1hp8d8a-0.sc-bpubUI.eKeNGz").text)
        except:
            data["name"].append(None)
        try:
            data["price"].append(j.find_element(By.CLASS_NAME, "sc-1hez2tp-0.sc-sVRsr.jIrGje").text)
        except:
            data["price"].append(None)
        try:
            data["cuisine"].append(j.find_element(By.CLASS_NAME, "sc-1hez2tp-0.sc-sVRsr.gVSfmH").text)
        except:
            data["cuisine"].append(None)
        try:
            data["location"].append(j.find_element(By.CLASS_NAME, "sc-1hez2tp-0.sc-jkPxnQ.jKgMiA").text)
        except:
            data["location"].append(None)
        try:
            data["link"].append(j.find_element(By.CLASS_NAME, "sc-btewqU.dlikcC").get_attribute("href"))
        except:
            data["link"].append(None)
        try:
            data["rating"].append(j.find_element(By.CLASS_NAME, "sc-1q7bklc-1.cILgox").text)
        except:
            data["rating"].append(None)

    time.sleep(3)

driver.quit()    # close the webpage
```
#### Step 5: Scrape Information for Each Product (Opening every product link and scrapping data)

```bash
#for link in data["link"]:
for url in data["link"]:
    driver.get(url) #OPEN URL 
    try:
        name_element = WebDriverWait(driver, 5).until(
            EC.presence_of_element_located((By.CSS_SELECTOR, ".sc-7kepeu-0.sc-iSDuPN.fwzNdh")) # WAIT UNTIL PRESENCE LOCATED
        )
        N = name_element.text if name_element else None    # GET THE TEXT IF FOUND ELSE NONE
    except:
        N = None
    time.sleep(4)    # SLEEP TO AVOID REJECTION FROM WEBSITE
    try:
        rating=driver.find_element(By.CSS_SELECTOR,".sc-dxZgTM.hOUWLw")# TARGET PARENT CLASSES
        delivery=rating.find_elements(By.CSS_SELECTOR,".sc-1q7bklc-8.kEgyiI") # TARGET SIBLING CLASSES
        d=delivery[1].text  # GET THE ELEMENT AT INDEX 1
    except:
        d= None

    try:            #SINCE THE CLASS OF DISH CHANGES FIND THE ANCHOR TAG WHICH HAS TEXT AS MENTIONOED AND IN THAT FIND THE SIBLING CLASS P WHICH HOLDS DISHES TEXT
        popular_element = WebDriverWait(driver, 4).until(
            EC.presence_of_element_located((By.XPATH, "//h3[text()='Popular Dishes']/following-sibling::p"))
        )
        dish = popular_element.text if popular_element else None  #GET THE TEXT IF FOUND ELSE NONE
    except:
        dish = None
    time.sleep(4)    # SLEEP TO AVOID REJECTION FROM WEBSITE
    try:           
        second_page["Timings"].append(driver.find_element(By.XPATH, "/html/body/div[1]/div/main/div/section[3]/section/section/div/div/section[2]/section/span[2]").text)
    except Exception as e:
        if '404' in str(e):
            second_page["Timings"].append(None)  # Append None if 404 error occurs
        else:
            second_page["Timings"].append(None)  # Append None for any other errors
        continue  # Continue with the next URL
  
    try:                  #SINCE THE CLASS OF DISH CHANGES FIND THE ANCHOR TAG WHICH HAS TEXT AS MENTIONOED AND IN THAT FIND THE SIBLING CLASS P WHICH HOLDS DISHES TEXT
        second_page["Resturant_known_for"].append(driver.find_element(By.XPATH, "/html/body/div[1]/div/main/div/section[4]/section/section/article[1]/section[1]/p[2]").text)   
    except:
        second_page["Resturant_known_for"].append(None)
    

    second_page["name"].append(N)                      #ADDING THE VALUES EXTRACTED
    second_page["delivery_review_number"].append(d)
    second_page["dish_name"].append(dish)
    #second_page["Timings"].append(time)

driver.quit()  # Close the browser after the loop


```
#### Step 6: Checking and Combining the Whole Data into a DataFrame for Cleaning 
```bash
#cleaning the data (price)
zomato_first_page["price"] = zomato_first_page["price"].str.replace(r'\D', '', regex=True).astype(int)
#(replacing the New with 0)
zomato_first_page.loc[zomato_first_page['rating'] == 'New', 'rating'] = 0
merged_zomato['restaurant_id'] = range(1, len(merged_zomato) + 1)  # ADD A COLUMN IN THE DATASET WITH VALUES PROVIDED IN THE RANGE
merged_zomato = pd.merge(zomato, zomato_2, on="name")  # MERGE THE MAIN PAGE DATA WITH SECOND PAGE DATA ON THE BASIS OF NAME
```

### 4. Data Cleaning

After scraping the data, it is essential to clean it to ensure accuracy and consistency. The following steps are performed for data cleaning:

#### Step 1: Handle Missing Values and Duplicate Records
```bash
merged_zomato = merged_zomato.dropduplicates()
```

#### Step 3: Save Cleaned Data to CSV
```bash
merged_zomato.to_csv("final_zomato.csv") # CONVERT THE CLEAN DATA TO CSV FILE FOR FURTHER ANALYSIS
```

### 5. Data Analysis

After scraping and cleaning the data, we proceeded to analyze it to extract meaningful insights.
The primary focus of our analysis was to understand the restaurant available on the Zomato platform. Here are the key steps and techniques we used in our data analysis:

#### Descriptive Statistics:

- Location Analysis: We calculated the total restaurant, average,  prices of restaurants for two persons.
- Rating Analysis: We analyzed the average ratings and the number of ratings for each restaurant to identify the most popular and highly-rated restaurants.
- Ingredient Analysis: We examined the locations of restaurants .

#### Grouping and Aggregation:

- Resturant known for: We grouped the restaurant to identify the number of cuisines available in each restaurant.
- Location Specialization: We analyzed which location is suitable for opening the restaurant.
- Resturant reviews : Number of restaurants  with reviews more than 1000.
  

#### Trend Analysis:

- We looked at the distribution of prices and ratings across different restaurants and benefit locations to identify.

Here, I'm including the document with the detailed insights we derived from our data analysis. 

Additionally, here are some graphs that we plotted to give you more visual insights. These graphs complement the visual insights available in the dashboard and provide a deeper understanding of the data.


### 6. Dashboard Creation
To make our analysis accessible and user-friendly, we created an interactive dashboard using visualization tools. The dashboard provides a comprehensive view of the restaurant on the Zomato Bangalore outlet.
Key features of the dashboard include:
- Interactive Filters
- Visual Representations
- Detailed Tables
- Insights

Here is a screenshot of the dashboard:


![image](https://github.com/user-attachments/assets/918156ee-9659-46a3-a01d-f627b35f711a)



The dashboard is designed to be an invaluable tool for consultancy firm, allowing them to explore the data dynamically and make informed decisions.



### 7. Conclusion
Our project successfully demonstrates the process of scraping, cleaning, analyzing, and visualizing data from a dynamic website. By leveraging Selenium for web scraping, Pandas and NumPy for data processing,
and interactive visualization tools for dashboard creation, we have created a comprehensive and insightful overview of the Bangalore outlets from the food delivery platform Zomato .

This project not only provides valuable insights in restaurants but also showcases the power of data analysis and visualization in making data-driven decisions.

We hope our work will be a valuable resource for anyone looking to explore the homeopathic medicine market. Thank you for your interest in our project. We look forward to any feedback or questions you may have.
