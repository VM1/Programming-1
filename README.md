

import time
import hashlib
import requests
import json
import random

min = 1
max = 99
randomgetal = random.randrange(min,max+1)

print("Random waarde:",randomgetal)


timestamp = str(time.time())
private_key = "950804e972fbe29b01d59e474c0381d76e4679aa"
public_key = "8666e4a8a1d32fbd47e02507170afa9c"

hash = hashlib.md5( (timestamp+private_key+public_key).encode('utf-8') )
md5digest = str(hash.hexdigest())

url = "http://gateway.marvel.com:80/v1/public/characters?limit=100&ts="+timestamp+"&apikey="+public_key+"&hash="+md5digest

response = requests.get(url)
jsontext = json.loads(response.text)


print("\nGevonden characters in comics:")
for item in jsontext['data']['results'][randomgetal]['comics']['items']: # deze volgorde kun je uit het zojuist geprinte resultaat halen of uit de Marvel documentatie!
    print(item['name'])

print("\nGevonden characters in series:")
for item in jsontext['data']['results'][randomgetal]['series']['items']: # deze volgorde kun je uit het zojuist geprinte resultaat halen of uit de Marvel documentatie!
    print(item['name'])
print('superheldnaam:')
print(jsontext['data']['results'][randomgetal]['name'])

hint1 = 'In deze comic is de held voorgekomen:', jsontext['data']['results'][randomgetal]['comics']['items'][0]

print(hint1)
