# -*- coding: utf-8 -*-
# @Time    : 2021/11/20 10:30
# @Author  : MENGYU R
# @File    : fzu_dinning_hall_analysis_project.py
# @Software IDE: PyCharm
import datetime
import time
import pymysql
import requests
import re

headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.106 Safari/537.36 Edg/80.0.361.54',
    'Host': 'www.fzu.edu.cn',
}


# Check that if the link can respond correctly and return response.text or response.status_code
def get_page(url):
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        return response.text
    else:
        return response.status_code


# Use numbers to represent each canteen
def modifyName(str):
    if str == "朝阳":
        return 1
    elif str == "丁香园1楼":
        return 2
    elif str == "丁香园2楼":
        return 3
    elif str == "禾丰元":
        return 4
    elif str == "京元":
        return 5
    elif str == "玫瑰园":
        return 6
    elif str == "紫荆园1楼":
        return 7
    elif str == "紫荆园2楼":
        return 8


# Use numbers to represent the intensity of the canteen
def modifyStatus(str):
    if str == "宽松":
        return 1
    elif str == "正常":
        return 2
    elif str == "拥挤":
        return 3


# Get the data of every dinning hall
def getData(html):
    for i in range(8):
        # get the name of the dining hall
        name_pattern = re.compile("roomName...(.{2,5})...fullNum", re.S)
        dinning_name = re.findall(name_pattern, html)[i]
        # print("data: ", modifyName(dinning_name), '\n', end='')

        # get the fullNum of the dining hall
        fullNum_pattern = re.compile("fullNum..([0-9]*)..peoNum", re.S)
        fullNum = re.findall(fullNum_pattern, html)[i]
        # print("fullNum: ", fullNum, '\n', end='')

        # get the peoNum of the dining hall
        peoNum_pattern = re.compile("peoNum..([0-9]*)..status", re.S)
        peoNum = re.findall(peoNum_pattern, html)[i]
        # print("peoNum: ", peoNum, '\n', end='')

        # get the status of the dining hall
        status_pattern = re.compile("status...(.{2})...em", re.S)
        status = re.findall(status_pattern, html)[i]
        # print("status: ", modifyStatus(status), '\n', end='')

        # get the em of the dining hall
        em_pattern = re.compile("em..([0-9]*)..closetime", re.S)
        em = re.findall(em_pattern, html)[i]
        # print("em: ", em, '\n', end='')

        # get the close time of the dining hall
        close_time_pattern = re.compile("closetime..([0-9]*)..incount", re.S)
        close_time = re.findall(close_time_pattern, html)[i]
        # print("close time: ", close_time, '\n', end='')

        # get the in_count of the dining hall
        in_count_pattern = re.compile("incount..([0-9]*)..outcount", re.S)
        in_count = re.findall(in_count_pattern, html)[i]
        # print("in count: ", in_count, '\n', end='')

        # get the out count of the dining hall
        out_count_pattern = re.compile("outcount..([0-9]*)}", re.S)
        out_count = re.findall(out_count_pattern, html)[i]
        # print("out count: ", out_count, '\n', end='')

        dt = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        # print(dt)
        # store in database
        sql = "insert into info(date_time, dinning_name, fullNum, peoNum, status, em, close_time, in_count, out_count) values(%s, %s, %s, %s, %s, %s, %s, %s, %s)"
        cursor.execute(sql, (dt, modifyName(dinning_name), fullNum, peoNum, modifyStatus(status), em, close_time, in_count, out_count))
        db.commit()


# get pages
def main():
    main_url = 'http://hqczhct.fzu.edu.cn:8001/Home/GetOnlineNumList'  # get every main page url
    html = get_page(main_url)  # get every page html
    # print(html)
    getData(html)


if __name__ == '__main__':
    # store in database
    db = pymysql.connect(host='***.**.**.***', user='root', password='*******', port=3306, db='fzu_info')
    cursor = db.cursor()
    cursor.execute("drop table if EXISTS info")  # if exists fzu_info table, use execute drop table

    # create database table
    sql_createTb = """CREATE TABLE info (
                     id INT NOT NULL AUTO_INCREMENT,
                     date_time TEXT,
                     dinning_name int,
                     fullNum int,
                     peoNum int,
                     status int,
                     em int,
                     close_time int,
                     in_count int,
                     out_count int,
                     PRIMARY KEY(id))
                     """
    cursor.execute(sql_createTb)

    while True:
        main()

        # get information every minute
        time.sleep(60)
