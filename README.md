# College-data
Web extraction of college data using python

## script ##

import requests
import re
import urllib
import xlsxwriter
import os
import validators
from bs4 import BeautifulSoup

k=['AB65$https://#############/university/25534-maharishi-markandeshwar-mmdu-mullana-campus-ambala/courses-fees','AE68$https://###########/college/56220-sri-ramachandra-medical-college-chennai/courses-fees']
workbook = xlsxwriter.Workbook('Test-04.xlsx')
worksheet= workbook.add_worksheet('Courses')

#### CODE ####
j=0
for index in range(len(k))
    m = str(k[index])
    url = m[m.rfind('$')+1:]
    lurl = m.split('$')
    code = lurl[0]
    print (code)
    agent = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36'}
    source= requests.get(url, headers=agent).text
    body= BeautifulSoup(source,"html.parser")
    for soup in body.find_all('div',{'class':'course_snipp_body new'}):
        # Top
        top = soup.find('div',{'class':'div-top'})
        
        # Streams
        n=13
        stream = soup.find('div',{'class':'div-stream-list content show-more-div'})
        try:
            for course in stream.find_all('a'):
                worksheet.write(j,n,course.text)
                n = n + 1
        except:
            worksheet.write(j,n,'ZZZZ')

        # Top - Left
        left = top.find('div',{'class':'left'})
        course = left.find('a')
        dur = left.find('span',{'class':'course_info duration-yr'})
        typ = left.find('span',{'class':'course_info duration-txt'})
        worksheet.write(j,0,code)
        worksheet.write(j,1,course.text)
        worksheet.write(j,2,dur.text)
        worksheet.write(j,3,typ.text)
        #print(course.text)
        #print(dur.text)
        #print(typ.text)

        # Top - Right
        right = top.find('div',{'class':'right'})
        try:
            fee = right.find('span',{'class':'fees'})
            #print(fee.text)
            worksheet.write(j,4,fee.text)
        except:
            fee = right.find('span',{'class':'fees'})
            #print(fee)
            worksheet.write(j,4,fee)
        try:
            term = right.find('span',{'class':'fees_per_yr'})
            #print(fee.text)
            worksheet.write(j,5,term.text)
        except:
            term = right.find('span',{'class':'fees_per_yr'})
            #print(fee)
            worksheet.write(j,5,term)
        #term = right.find('span',{'class':'fees_per_yr'})
        #worksheet.write(j,5,term.text)
        #print(term.text)

        # Fees Break Up
        t=6
        brkup = soup.find('div',{'class':'width_maintain'})
        for head in brkup.find_all('tr'):
            #for th in head.find_all('th'):
                #worksheet.write(j,t,th.text)
                #t = t + 1
                #print(th.text)
            for td in head.find_all('td'):
                worksheet.write(j,t,td.text)
                t = t + 1
                #print(td.text)
        #print('--')
        j = j + 1
workbook.close()
