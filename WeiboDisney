# -*- coding: UTF-8 -*-

import sys
import time
import datetime
import re
import os
import sys
import codecs
import shutil
import urllib
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import selenium.webdriver.support.ui as ui
from selenium.webdriver.common.action_chains import ActionChains

reload(sys)
sys.setdefaultencoding("utf-8")
driver = webdriver.Chrome()
wait = ui.WebDriverWait(driver, 10)

infoWeibo =codecs.open("SinaWeibo_Info.txt", 'a', 'utf-8')

def LoginWeibo(username, password):
    try:
        print '准备登陆Weibo.cn网站...'
        driver.get("http://login.weibo.cn/login/")
        elem_user = driver.find_element_by_name("mobile")
        elem_user.send_keys(username)  # 用户名
        elem_pwd = driver.find_element_by_xpath("/html/body/div[2]/form/div/input[2]")
        elem_pwd.send_keys(password)  # 密码
        # elem_rem = driver.find_element_by_name("remember")
        # elem_rem.click()             #记住登录状态

        # 重点: 暂停时间输入验证码
        # pause(millisenconds)
        time.sleep(15)

        elem_sub = driver.find_element_by_name("submit")
        elem_sub.click()  # 点击登陆
        time.sleep(2)

        # 获取Coockie 推荐 http://www.cnblogs.com/fnng/p/3269450.html
        print driver.current_url
        print driver.get_cookies()  # 获得cookie信息 dict存储
        print u'输出Cookie键值对信息:'
        for cookie in driver.get_cookies():
            # print cookie
            for key in cookie:
                print key, cookie[key]

                # driver.get_cookies()类型list 仅包含一个元素cookie类型dict
        print u'登陆成功...'


    except Exception, e:
        print "Error: ", e
    finally:
        print 'End LoginWeibo!\n\n'


        # ********************************************************************************

def Search(keywords):
    try:
        driver.get("http://weibo.cn/search/")
        element_advanced = driver.find_element_by_xpath("/html/body/div[4]/a[3]")
        element_advanced.click()
        time.sleep(2)
        current_window_1 = driver.current_window_handle
        print current_window_1
        element_search = driver.find_element_by_xpath("/html/body/div[6]/form/div/input[2]")
        element_search.send_keys(keywords)
        element_search_Vusers = driver.find_element_by_xpath("/html/body/div[6]/form/div/input[8]")
        element_search_Vusers.click()
        element_search_stime = driver.find_element_by_xpath("/html/body/div[6]/form/div/input[11]")
        element_search_stime.send_keys("20160613")
        element_search_etime = driver.find_element_by_xpath("/html/body/div[6]/form/div/input[12]")
        element_search_etime.clear()
        time.sleep(1)
        element_search_etime.send_keys("20160619")
        element_search_commit = driver.find_element_by_xpath("/html/body/div[6]/form/div/input[last()]")
        element_search_commit.click()
        time.sleep(2)
        current_window_2 = driver.current_window_handle
        print current_window_2
        print "开始抓取微博....\n"
        #+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
        infoWeibo.write("微博内容: ".decode('utf-8') + "\r\n\r\n")
        element_search_page1 = driver.find_element_by_xpath("/html/body/div[@class='pa']/form/div/a[1]")
        element_search_page1.click()
        current_window_4 = driver.current_window_handle
        print current_window_4
        element_search_page2 = driver.find_element_by_xpath("/html/body/div[@class='pa']/form/div/a[1]")
        element_search_page2.click()
        current_window_5 = driver.current_window_handle
        print current_window_5
        #info = driver.find_elements_by_xpath("//div[@class='c']")
        removeKey_1 = "收藏"
        removeKey_2 = "月"
        removeKey_3 = "日"
        removeKey_4 = "设置:皮肤.图片.条数.隐私"
        # removeKey_5 = "返回我的首"
        num = 1
        while num <= 100:
            info = driver.find_elements_by_xpath("//div[@class='c']")
            for value in info:
                #print value.text
                info_1 = value.text
                if removeKey_1.decode("utf-8") and removeKey_2.decode("utf-8") and removeKey_3.decode(
                        "utf-8") in info_1:
                    print info_1
                    info_2 = info_1.split(' 收藏 ')[-1]
                    print info_2
                    # flag_1 = info_1.find(' 来自'.decode('utf-8'))
                    # info_time = info_2[:flag_1]
                    flag_2 = info_2.find('日 ')
                    flag_3 = info_2[:flag_2]
                    print flag_3
                    info_mon = flag_3.split('月')[0]
                    info_date = flag_3.split('月')[-1]
                    print 'hhhh'
                    print str(info_mon).decode('utf-8')
                    flag_4 = '2016-'.decode('utf-8') + str(info_mon).decode('utf-8') + '-'.decode('utf-8') + str(
                        info_date).decode('utf-8')
                    print flag_4
                    date_flag_4 = datetime.datetime.strptime(flag_4, '%Y-%m-%d')
                    date_flag_5 = datetime.datetime.strptime('2016-6-13', '%Y-%m-%d')
                    date_flag_6 = datetime.datetime.strptime('2016-6-19', '%Y-%m-%d')
                    if (date_flag_4 <= date_flag_6) and (date_flag_4 >= date_flag_5):
                        infoWeibo.write(str(info_1).decode('utf-8') + "\r\n\r\n")
                else:
                    print '跳过'
            else:
                print "next page\n"
            element_search_page = driver.find_element_by_xpath("/html/body/div[@class='pa']/form/div/a[1]")
            element_search_page.click()
            time.sleep(1)
            current_window_3 = driver.current_window_handle
            print current_window_3
            num += 1
    except Exception, e:
        print "error: ", e
    finally:
        print u'End searching Weibo'


if __name__ == '__main__':

    username = 'tianfan*******'  # 输入你的用户名
    password = '1******'  # 输入你的密码

    LoginWeibo(username, password)

    # driver.add_cookie({'name':'name', 'value':'_T_WM'})
    # driver.add_cookie({'name':'value', 'value':'c86fbdcd26505c256a1504b9273df8ba'})

    # 注意
    # 因为sina微博增加了验证码,但是你用Firefox登陆一次输入验证码,再调用该程序即可,因为Cookies已经保证
    # 会直接跳转到明星微博那部分,即: http://weibo.cn/guangxianliuyan
    words = "#上海迪士尼#"
    print words
    Search(words.decode('utf-8'))
