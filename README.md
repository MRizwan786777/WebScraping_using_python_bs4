# WebScraping_using_python_bs4
#In this project we will be making functions which will scrap indeed first 4 pages from 0-4 and check for python developers #jobs title, company name and salary and at the end make an csv file and store everything there using pandas data frame. 
import requests
from bs4 import BeautifulSoup
import pandas as pd


def extract(page):
    headers = {'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.71 Safari/537.36'} 
    url = 'https://pk.indeed.com/jobs?q=python+developer&l=Islamabad&start={page}'
    r = requests.get(url,headers)
    soup = BeautifulSoup(r.content,'html.parser')
    return soup

def transfrom(soup):
    divs = soup.find_all('div', class_ = 'job_seen_beacon')
    for item in divs:
        title = item.find('h2').text.strip()
        company = item.find('span', class_ = 'companyName').text.strip()
        try:
            salary = item.find('span', class_ = 'metadata salary-snippet-container').text.strip()
        except:
            salary = ''
        
        jobs = {
            'title':title,
            'company':company,
            'salary': salary
        }
        joblist.append(jobs)
    return

joblist = []  

for i in range(0,40,10):
    print(f'Getting page, {i}')
    c = extract(0)
    transfrom(c)

df = pd.DataFrame(joblist)

print(df.head())
df.to_csv('jobsfromindeed.csv')
