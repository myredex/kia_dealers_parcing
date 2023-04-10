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
    'AMCVS_5288FC7C5A0DB1AD0A495DAA%40AdobeOrg': '1',
    'at_check': 'true',
    '_gcl_au': '1.1.1115191399.1680607834',
    'server-id': '2',
    'AMCV_5288FC7C5A0DB1AD0A495DAA%40AdobeOrg': '179643557%7CMCIDTS%7C19452%7CMCMID%7C66567593446243994910443663988301828962%7CMCAAMLH-1681212633%7C6%7CMCAAMB-1681212633%7C6G1ynYcLPuiQxYZrsz_pkqfLG9yMXBpb2zX5dvJdYQJzPXImdj0y%7CMCOPTOUT-1680615033s%7CNONE%7CMCSYNCSOP%7C411-19459%7CvVersion%7C5.5.0',
    'mboxEdgeCluster': '37',
    'adobe_visits': '1',
    '_gid': 'GA1.2.1502647912.1680607835',
    '_scid': '75c7f989-6d40-4710-9b9c-a28bd1e6830d',
    '_fbp': 'fb.1.1680607836597.405262138',
    '_tt_enable_cookie': '1',
    '_ttp': '-eY47kRr1slqeN6OtFllF7rC9aY',
    '_hjFirstSeen': '1',
    '_hjIncludedInSessionSample_1637272': '0',
    '_hjSession_1637272': 'eyJpZCI6IjEzOThmNDMzLTUwMGMtNDcwMS04NWIxLTM3OGQ0MjYxMzg5ZSIsImNyZWF0ZWQiOjE2ODA2MDc4MzcwMjgsImluU2FtcGxlIjpmYWxzZX0=',
    '_hjAbsoluteSessionInProgress': '0',
    '_pin_unauth': 'dWlkPVlXSTJOalExTW1RdE5XWTJZeTAwTVdOa0xXRm1Namt0TVdFek9XTmlaamt6TVRsbA',
    '_sctr': '1|1680544800000',
    '__insp_wid': '192154059',
    '__insp_nv': 'true',
    '__insp_targlpu': 'aHR0cHM6Ly93d3cua2lhLmNvbS91cy9lbg%3D%3D',
    '__insp_targlpt': 'U1VWcywgU2VkYW5zLCBTcG9ydHMgQ2FycywgSHlicmlkcywgRVZzICYgTHV4dXJ5IENhcnMgfCBLaWE%3D',
    's_fid': '5A47E62700B0D53A-1925E3970202B33E',
    's_vnum': '1682877600993%26vn%3D1',
    's_invisit': 'true',
    's_cc': 'true',
    '__insp_norec_sess': 'true',
    '_aeaid': '7eb7fb30-1422-4452-b20d-e20b6221efa9',
    's_dfa': 'hkmkiatier1dev%2Chkmkiatier1prod',
    'aelastsite': '7JVBFOlghlAorlmnXsY44CAxi1yp%2FEG78H%2Bq39Ppe74Z9mc4yUqpU6BTW%2B8H8LrF',
    'aelreadersettings': '%7B%22c_big%22%3A0%2C%22rg%22%3A0%2C%22memph%22%3A0%2C%22contrast_setting%22%3A0%2C%22colorshift_setting%22%3A0%2C%22text_size_setting%22%3A0%2C%22space_setting%22%3A0%2C%22font_setting%22%3A0%2C%22k%22%3A0%2C%22k_disable_default%22%3A0%2C%22hlt%22%3A0%2C%22disable_animations%22%3A0%2C%22display_alt_desc%22%3A0%7D',
    '_hjSessionUser_1637272': 'eyJpZCI6ImRiYzBjMWRjLTdkMjEtNTY1ZC1hOTFjLTEzNTkxYzBjZjlmNiIsImNyZWF0ZWQiOjE2ODA2MDc4MzcwMTAsImV4aXN0aW5nIjp0cnVlfQ==',
    's_pp': 'find-a-dealer%3A%20result',
    's_sq': '%5B%5BB%5D%5D',
    '_uetsid': '1e8a34b0d2dc11eda1de093d2660ff8f',
    '_uetvid': '1e8aed80d2dc11eda8bf55d6301eb03f',
    's_ppvl': 'https%253A%2F%2Fwww.kia.com%2Fus%2Fen%2Ffind-a-dealer%2Fresult%253FzipCode%253D0%2C33%2C33%2C297%2C1392%2C297%2C1440%2C900%2C2%2CP',
    '__insp_slim': '1680608667373',
    's_ppv': 'https%253A%2F%2Fwww.kia.com%2Fus%2Fen%2Ffind-a-dealer%2Fresult%253FzipCode%253D0%2C54%2C32%2C497%2C1392%2C297%2C1440%2C900%2C2%2CP',
    'no_aa': 'true',
    'RT': '"z=1&dm=kia.com&si=39bda5ef-7588-4bcf-bfd2-b214427e4cc8&ss=lg26ieei&sl=6&tt=jrd&bcn=%2F%2F0217991a.akstat.io%2F"',
    'utag_main': 'v_id:01874c0872d80019ae195f5bd20b0508b00190830093c$_sn:1$_ss:0$_st:1680610596593$ses_id:1680607834843%3Bexp-session$_pn:7%3Bexp-session$vapi_domain:kia.com',
    'mbox': 'session#62d62ca80c9b4f95b0bbd03e00d56b3f#1680609695|PC#62d62ca80c9b4f95b0bbd03e00d56b3f.37_0#1743853597',
    '_ga': 'GA1.1.1802862933.1680607835',
    '_ga_V2C03MRXK2': 'GS1.1.1680607834.1.1.1680608797.58.0.0',
    'zipCode': '10012',
}

headers = {
    'authority': 'www.kia.com',
    'accept': 'application/json, text/plain, */*',
    'accept-language': 'en,la;q=0.9,ru;q=0.8,kk;q=0.7',
    'content-type': 'application/json;charset=UTF-8',
    # 'cookie': '_schn=_h2rakk; AMCVS_5288FC7C5A0DB1AD0A495DAA%40AdobeOrg=1; at_check=true; _gcl_au=1.1.1115191399.1680607834; server-id=2; AMCV_5288FC7C5A0DB1AD0A495DAA%40AdobeOrg=179643557%7CMCIDTS%7C19452%7CMCMID%7C66567593446243994910443663988301828962%7CMCAAMLH-1681212633%7C6%7CMCAAMB-1681212633%7C6G1ynYcLPuiQxYZrsz_pkqfLG9yMXBpb2zX5dvJdYQJzPXImdj0y%7CMCOPTOUT-1680615033s%7CNONE%7CMCSYNCSOP%7C411-19459%7CvVersion%7C5.5.0; mboxEdgeCluster=37; adobe_visits=1; _gid=GA1.2.1502647912.1680607835; _scid=75c7f989-6d40-4710-9b9c-a28bd1e6830d; _fbp=fb.1.1680607836597.405262138; _tt_enable_cookie=1; _ttp=-eY47kRr1slqeN6OtFllF7rC9aY; _hjFirstSeen=1; _hjIncludedInSessionSample_1637272=0; _hjSession_1637272=eyJpZCI6IjEzOThmNDMzLTUwMGMtNDcwMS04NWIxLTM3OGQ0MjYxMzg5ZSIsImNyZWF0ZWQiOjE2ODA2MDc4MzcwMjgsImluU2FtcGxlIjpmYWxzZX0=; _hjAbsoluteSessionInProgress=0; _pin_unauth=dWlkPVlXSTJOalExTW1RdE5XWTJZeTAwTVdOa0xXRm1Namt0TVdFek9XTmlaamt6TVRsbA; _sctr=1|1680544800000; __insp_wid=192154059; __insp_nv=true; __insp_targlpu=aHR0cHM6Ly93d3cua2lhLmNvbS91cy9lbg%3D%3D; __insp_targlpt=U1VWcywgU2VkYW5zLCBTcG9ydHMgQ2FycywgSHlicmlkcywgRVZzICYgTHV4dXJ5IENhcnMgfCBLaWE%3D; s_fid=5A47E62700B0D53A-1925E3970202B33E; s_vnum=1682877600993%26vn%3D1; s_invisit=true; s_cc=true; __insp_norec_sess=true; _aeaid=7eb7fb30-1422-4452-b20d-e20b6221efa9; s_dfa=hkmkiatier1dev%2Chkmkiatier1prod; aelastsite=7JVBFOlghlAorlmnXsY44CAxi1yp%2FEG78H%2Bq39Ppe74Z9mc4yUqpU6BTW%2B8H8LrF; aelreadersettings=%7B%22c_big%22%3A0%2C%22rg%22%3A0%2C%22memph%22%3A0%2C%22contrast_setting%22%3A0%2C%22colorshift_setting%22%3A0%2C%22text_size_setting%22%3A0%2C%22space_setting%22%3A0%2C%22font_setting%22%3A0%2C%22k%22%3A0%2C%22k_disable_default%22%3A0%2C%22hlt%22%3A0%2C%22disable_animations%22%3A0%2C%22display_alt_desc%22%3A0%7D; _hjSessionUser_1637272=eyJpZCI6ImRiYzBjMWRjLTdkMjEtNTY1ZC1hOTFjLTEzNTkxYzBjZjlmNiIsImNyZWF0ZWQiOjE2ODA2MDc4MzcwMTAsImV4aXN0aW5nIjp0cnVlfQ==; s_pp=find-a-dealer%3A%20result; s_sq=%5B%5BB%5D%5D; _uetsid=1e8a34b0d2dc11eda1de093d2660ff8f; _uetvid=1e8aed80d2dc11eda8bf55d6301eb03f; s_ppvl=https%253A%2F%2Fwww.kia.com%2Fus%2Fen%2Ffind-a-dealer%2Fresult%253FzipCode%253D0%2C33%2C33%2C297%2C1392%2C297%2C1440%2C900%2C2%2CP; __insp_slim=1680608667373; s_ppv=https%253A%2F%2Fwww.kia.com%2Fus%2Fen%2Ffind-a-dealer%2Fresult%253FzipCode%253D0%2C54%2C32%2C497%2C1392%2C297%2C1440%2C900%2C2%2CP; no_aa=true; RT="z=1&dm=kia.com&si=39bda5ef-7588-4bcf-bfd2-b214427e4cc8&ss=lg26ieei&sl=6&tt=jrd&bcn=%2F%2F0217991a.akstat.io%2F"; utag_main=v_id:01874c0872d80019ae195f5bd20b0508b00190830093c$_sn:1$_ss:0$_st:1680610596593$ses_id:1680607834843%3Bexp-session$_pn:7%3Bexp-session$vapi_domain:kia.com; mbox=session#62d62ca80c9b4f95b0bbd03e00d56b3f#1680609695|PC#62d62ca80c9b4f95b0bbd03e00d56b3f.37_0#1743853597; _ga=GA1.1.1802862933.1680607835; _ga_V2C03MRXK2=GS1.1.1680607834.1.1.1680608797.58.0.0; zipCode=10012',
    'custom-spinner': 'true',
    'origin': 'https://www.kia.com',
    'referer': 'https://www.kia.com/us/en/find-a-dealer/result?zipCode=10012',
    'sec-ch-ua': '"Chromium";v="110", "Not A(Brand";v="24", "Yandex";v="23"',
    'sec-ch-ua-mobile': '?0',
    'sec-ch-ua-platform': '"Windows"',
    'sec-fetch-dest': 'empty',
    'sec-fetch-mode': 'cors',
    'sec-fetch-site': 'same-origin',
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.0.0 YaBrowser/23.3.0.2247 Yowser/2.5 Safari/537.36',
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
