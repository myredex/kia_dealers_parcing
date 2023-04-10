# Collect dealers from Kia.com

With this notebook you will be able to parce information from Kia.com and other websites.
- First go to https://www.kia.com/us/en/find-a-dealer/result?zipCode=10011
- Right click on the page and open "Inspect element"
- Go to "Network" tab
- Reload the page (F5)
- Press Fetch/XHR button and find name "search" once you press it you will see json response [{"location":{"street1":"46-05 Northern Blvd ....]
- Click Right button on "search" then "copy" then "as cURL (bash)" 
- Go to https://curlconverter.com/ and paste to the field
- Choose Python and copy cookies and headers
- Paste it below

## First we need to get US zip codes


```python
# !pip install uszipcode
```


```python
from uszipcode import SearchEngine

engine = SearchEngine()

# Set returns to 20000000000 
zipcodes = engine.by_population(lower = 50000, returns=5)

zipcode_list = []
for zipcode in zipcodes:
    zipcode_list.append(zipcode.zipcode)
    # print(zipcode.zipcode, zipcode.city, zipcode.major_city, zipcode.population)
len(zipcode_list)
```




    5



## Now It's time to make request


```python
import requests
```


```python
# Don't forget paster your cookies and headers to make your code work

cookies = {
    '_schn': '_h2rakk',
}

headers = {
    'authority': 'www.kia.com',
    'accept': 'application/json, text/plain, */*',
}
```


```python
# Import libraries
import json
import time
import pandas as pd
# Turn off warnings
import warnings
warnings.simplefilter(action='ignore', category=FutureWarning)
# Progress bar
from tqdm.notebook import tqdm

# Create blank dataframe
df = pd.DataFrame(columns=['company_name', 'phone', 'phone2', 'adress', 'city', 
                           'zip_code', 'state', 'website', 'email','coord'])

for zip in tqdm(zipcode_list):

    json_data = {
        'type': 'zip',
        'zipCode': zip,
        'dealerCertifications': [],
        'dealerServices': [],
    }
    
    # Wait 1 second and make request
    time.sleep(1)
    response = requests.post('https://www.kia.com/us/services/en/dealers/search', 
                             cookies=cookies, 
                             headers=headers, 
                             json=json_data)
    
    # Get result and parce it as json
    result = json.loads(response.text)
    
    for company in result:

        company_name = company['name']
        company_phone = company['phones'][0]['number']
        company_website = company['url']
        company_adress = company['location']['street1']
        company_city = company['location']['city']
        company_state = company['location']['state']
        zip_code = company['location']['zipCode']
        company_loc = company['location']['latitude'] + ", " + company['location']['longitude']
        
        # Append dataframe with information
        df = df.append({"company_name":company_name, 
                        "phone":company_phone, 
                        #"phone2":phone2, 
                        "adress":company_adress, 
                        "city":company_city, 
                        "zip_code":zip_code, 
                        "state":company_state, 
                        "website":company_website, 
                        "coord":company_loc}, ignore_index=True)
```


      0%|          | 0/5 [00:00<?, ?it/s]



```python
# Drop duplicates and save results
df = df.drop_duplicates()
df.to_csv('kia_dealers_1.csv', index=False)
```


```python

```
