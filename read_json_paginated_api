import requests
import json
import pandas as pd
from pandas import json_normalize
from datetime import datetime
from string import digits

url = 'https://abc.net/job/search'
headers = {
    'Authorization':'Token 1',
    'Content-Type':'application/json'
}
params={
    'process':'process1',
    'issued':'all', 
}

def getUsersList(results):
    for index in range(len(results)):
        df_json = pd.json_normalize(results)
    return df_json

response = requests.request("GET", url, headers=headers, params=params)
if response.status_code != 200:
    print(f'Error with status code: {response.status_code}')
    exit()


# Start with an empty list
total_results = []

## Download the first page
response_json = response.json()
if (len(response_json['results']) == 0):
    print('no data coming')
# Store the first page of results
total_results =  response_json['results']
df = pd.DataFrame()   
df = df.append(getUsersList(total_results))

print('Next response: ', response_json.get('next'))               
while response_json['next'] is not None:
    new_url = response_json['next']
    page_num = ''.join(val for val in new_url.split('page=')[1][0:3] if val in digits)
    print('Next URL: {} and Page Number:{}'.format(new_url, page_num))
    response = requests.request("GET", new_url,headers = headers)
    if response.status_code != 200:
        print(f'Error with status code: {response.status_code}')
        exit()
    response_json = response.json()
    # Store the current page of results
    total_results = total_results + response_json['results']
    df = df.append(getUsersList(total_results))

print("We have", len(total_results), "total results")
