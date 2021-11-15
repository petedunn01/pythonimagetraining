"# pythonimagetraining" 
import requests
from bs4 import BeautifulSoup
import os


#url = 'https://www.airbnb.com/rooms/37414586/photos/849969536?adults=2&federated_search_id=c90c7c72-7630-4714-a18d-5fb641c2d5a5&source_impression_id=p3_1636938940_DTtm818E8GHGC0wK&guests=1'

def imagedown(url, folder):
    try:
        os.mkdir(os.path.join(os.getcwd(), folder))  # create directory to join current directory and new folder
    except:
        pass

    # try/except to prevent crashing
    os.chdir((os.path.join(os.getcwd(), folder)))
    r = requests.get(url)
    soup = BeautifulSoup(r.text, 'html.parser')  # create the soup
    images = soup.find_all('img')

    # for image in images:
    # print(image['src'])     -- this would view the links to the images only without the 'alt' which provides text

    for image in images:
        name = image['alt']
        link = image['src']
        with open(name.replace(' ', '-').replace('/', '') + '.jpg', 'wb') as f:
            im = requests.get(link)
            f.write(im.content)
            print('Writing: ', name)


imagedown('https://www.airbnb.com/rooms/41001945?adults=2&check_in=2021-11-17&check_out=2021-11-19&previous_page_section_name=1000&federated_search_id=8ca74762-777d-4647-9626-5e9b40586788', 'image_folder')
