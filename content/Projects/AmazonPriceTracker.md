---
navigation.title: 'AmazonPriceTracker Python Project'
layout: 'full-width'
head.description: 'Learn how to track the price of an Amazon product and get email alerts when the price drops below a certain threshold using Python.'
---

# AmazonPriceTracker Python Project

In this project, we will create a Python script to track the price of an Amazon product and send an email notification if the price drops below a certain threshold.

Setting up the Project Environment
Install Python on your computer if you haven't already. You can download Python from the .

Create a virtual environment using venv or virtualenv. This will help you keep the project dependencies separate from your system's Python installation. You can create a virtual environment using the following command in your terminal:

python -m venv env
Activate the virtual environment using the following command:


source env/bin/activate
Install the required libraries using the following command:

pip install requests beautifulsoup4 pandas smtplib
Writing the Python Code
Import the required libraries:

import requests
from bs4 import BeautifulSoup
import pandas as pd
import smtplib
Define the URL of the product you want to track and extract the HTML content using the requests library:

URL = 'https://www.amazon.com/dp/B08J65CPS5/'
headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3"}
page = requests.get(URL, headers=headers)
Parse the HTML content using BeautifulSoup and extract the relevant information (product name, current price, and availability):


soup = BeautifulSoup(page.content, 'html.parser')
title = soup.find(id="productTitle").get_text().strip()
price = float(soup.find(id="priceblock_ourprice").get_text().replace('$', ''))
availability = soup.find(id="availability").get_text().strip()
Store the extracted data in a pandas DataFrame:


data = {'Product Name': [title], 'Price': [price], 'Availability': [availability]}
df = pd.DataFrame(data)
Define the email sender and receiver, and create a function to send the email:


sender_email = 'sender@gmail.com'
receiver_email = 'receiver@gmail.com'
password = 'yourpassword'

def send_email(subject, body):
    message = f'Subject: {subject}\n\n{body}'

    with smtplib.SMTP('smtp.gmail.com', 587) as server:
        server.starttls()
        server.login(sender_email, password)
        server.sendmail(sender_email, receiver_email, message)
Define the target price and send an email if the current price is lower than the target price:


target_price = 500

if price < target_price:
    subject = f'Price Alert: {title}'
    body = f'The price of {title} has dropped to ${price}. Buy now!'
    send_email(subject, body)