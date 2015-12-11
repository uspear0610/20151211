max 값 구하기
# 사용할 어플리케이션 복사
# cp /usr/local/web2py/applications/app1 /usr/local/web2py/applications/max -rf
# cd /usr/local/web2py/applications/max/controllers
# vim test.py

# metric : temperature
# tag : id
# id : 1
# -*- coding: utf-8 -*-

import datetime
import urllib2
import json
import time

url = "http://127.0.0.1:4242/api/query?start=1m-ago&m=sum:temperature%7Bid=1%7D&o=&yrange=%5B0:%5D&key=out%20bottom%20center%20box&wxh=740x345&autoreload=15"

위의 url을 아래의 url로 수정

url = "http://125.7.128.52:4242/api/query?start=6h-ago&m=sum:gyu_RC1_thl.temperature%7Bnodeid=2303%7D&o=&yrange=%500:%5d&wxh=1099x453&autoreload=15"

def make_data(raw_data):
        max_val = 0

        tmp_data = raw_data.replace('u' , '')
        tmp_data = raw_data.replace('{' , '')
        tmp_data = raw_data.replace("'" , '')
        tmp_data = raw_data.replace('}' , '')
        tmp_data = raw_data.split(',')

        for i in range(0, len(tmp_data)-1) :
                arr_data = tmp_data[i].split(':')
                arr_Time = arr_data[0].strip()
                arr_Value = arr_data[1].strip()

                if float(arr_Value) > float(max_val):
                        max_val = arr_Value
        return max_val

def test():
        max_value = 0
        max_time = 0
        param = request.vars['id']

        url_lib=urllib2.urlopen(url)
        url_data=url_lib.read()

        Data=json.loads(url_data)

        raw_data=str(Data[0]["dps"])
        result_max_val = make_data(raw_data)

        return result_max_val
