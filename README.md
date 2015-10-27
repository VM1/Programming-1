import time
import hashlib
import requests
import json
import random
from tkinter import *
from tkinter.messagebox import showinfo


##Random getal genereren
min = 1
max = 99
randomgetal = random.randrange(min,max+1)


##API inladen
timestamp = str(time.time())
private_key = "950804e972fbe29b01d59e474c0381d76e4679aa"
public_key = "8666e4a8a1d32fbd47e02507170afa9c"
hash = hashlib.md5( (timestamp+private_key+public_key).encode('utf-8') )
md5digest = str(hash.hexdigest())
url = "http://gateway.marvel.com:80/v1/public/characters?limit=100&ts="+timestamp+"&apikey="+public_key+"&hash="+md5digest
response = requests.get(url)
jsontext = json.loads(response.text)


##superheldnaam
superheldnaam = jsontext['data']['results'][randomgetal]['name']
print(superheldnaam)

##Hints
try:
    hint1 = 'In deze comic is de held voorgekomen:', jsontext['data']['results'][randomgetal]['comics']['items'][0]
    hint2 = 'In deze comic is de held voorgekomen:', jsontext['data']['results'][randomgetal]['comics']['items'][1]
    hint3 = 'In deze serie is de held voorgekomen:', jsontext['data']['results'][randomgetal]['series']['items'][0]
    print(hint1)
    print(hint2)
    print(hint3)
except:
    print('error, geen hints mogelijk')


##GUI Scherm
standaard_window = Tk()

def achtergrond():
    standaard_window
    photo=PhotoImage(file='rsz.gif')
    label = Label(standaard_window,image=photo)
    label.pack()
    standaard_window.mainloop()

topframe= Frame(standaard_window)
topframe.pack()
bottomframe=Frame(standaard_window)
bottomframe.pack(side=BOTTOM)
sideframe= Frame(standaard_window)
sideframe.pack(side=LEFT)
sideframe1=Frame(standaard_window)
sideframe1.pack(side=RIGHT)

label = Label(topframe,text='Raad de volgende Marvel-held!',bg='red',fg='white',font=('impact',35))
label.pack()


##GUI Hints
def random_hint():
    showinfo(title='Klik hier voor een hint',message='Hier komt de "random" hint')

button2 = Button(standaard_window, text= 'Hints!',font=('courier',15),bg='red', command=random_hint)
button2.pack(padx=50,pady=15)
button2.config(height=2,width=15)


##GUI Antwoord
invoer_antwoord = Entry(bottomframe)
invoer_antwoord.pack(pady=5)

def antwoord_goed_fout():
    if button_antwoord == superheldnaam:
        showinfo(message='victory')
    else:
        showinfo(message='Fout, score -1')

button_antwoord = Button(bottomframe,text = 'OK',font=('courier',15),command=antwoord_goed_fout)
button_antwoord.pack(pady=15)
button_antwoord.config(height=1,width=5)

achtergrond()
standaard_window.mainloop()

