# WebScrapping multiple pages from website:
## In this project we will show how to scrape information from multiple pages from website.

Extracted Information:
- Job title
- Company Name
- Location
- Job Type
- Job url

## This code will extract the below information using Python Code and will save the output in CSV file:

# Code	
## load needed libraries

	import requests
	from bs4 import BeautifulSoup
	import pandas as pd

## Link of website website (Job Search Website):
https://wuzzuf.net/search/jobs/?a=hpb&q=python&start=0

## Finding needed informatiob by inspecting its HTML tag/ID:
## Creating empty DataFrame Headers:
	df = pd.DataFrame(columns=['job_title','company_name','location','job_type','url'])

## Creating For Loop to iterate over each job's content and scrape the needed info. and save it in a list.
## will scrape pages from [1:3] (first 3 pages)
	for x in range(3):
## Make a request to a web page, to get the HTML content, but in this case will make loop for pages number needed to scrape
		url = requests.get('https://wuzzuf.net/search/jobs/?a=hpb&q=python&start='+str(x))
## Parse HTML Code With Beautiful Soup
		soup = BeautifulSoup(url.content,'html.parser')
		jobs = soup.find_all('div',class_="css-pkv5jc")
		for job in jobs:
			job_title = job.find('h2').text.strip()
			company_name = job.find('div',class_="css-d7j1kk").find(class_="css-17s97q8").text.replace('-','').strip()
			location = job.find('div',class_="css-d7j1kk").find(class_="css-5wys0k").text.strip()
			try:
			    posted_date = job.find('div',class_="css-4c4ojb").text.strip()
			except:
			    posted_date = job.find('div',class_="css-do6t5g").text.strip()
			job_type = job.find('a',class_="css-n2jc4m").text.strip()
			url = 'https://wuzzuf.net'+job.find('h2').find('a').get('href')
## append the extracted information to the created dataframe
			df = df.append({'job_title':job_title,'company_name':company_name,'location':location,'job_type':job_type,'url':url}
                       	,ignore_index=True)
    
## Save the output to CSV file in your local machine path:
	df.to_csv('D://Webscrapping/WebScrapping_Wuzzuf_Jobs_multiple_pages.csv',index=False)## Output
### DataFrame output:
![image](https://user-images.githubusercontent.com/109844538/180598932-4cc1edaf-3df4-4ad4-a3c7-6a2a299341d3.png)

### Newly Ceated CSV file:
![image](https://user-images.githubusercontent.com/109844538/180598973-4f916d42-135d-48b7-b845-bcc5c071132c.png)
