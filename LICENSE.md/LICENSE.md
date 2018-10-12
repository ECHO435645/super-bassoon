#-*- coding:UTF-8 -*-
import tkinter as tk
import requests
import json
#构造界面化GUI，设置title,界面大小
window = tk.Tk()
window.title('翻译小软件')
window.geometry('300x200')
# 构造Label，并设置位置
label1 = tk.Label(window,text='输入单词:',font='微软雅黑')
label1.place(x=10,y=30)

label2 = tk.Label(window,text='翻译结果:',font='微软雅黑')
label2.place(x=10,y=90)
# 创建变量，输入的和翻译的
var_input_words = tk.StringVar()
var_translate_words = tk.StringVar()
#输入框
tk.Entry(window,textvariable= var_input_words).place(x=100,y=35)
tk.Entry(window,textvariable= var_translate_words).place(x=100,y=95)
# 爬取有道翻译，查看源代码，发现发送的post/ajax请求 在preview中有我们需要的信息是json形式的。
def get_translate():
    content = var_input_words.get()
    # 构造headers,让程序看起来像是人为的操作
    headers={
        'Host':'fanyi.youdao.com',
        'Origin':'http://fanyi.youdao.com',
        'Referer':'http://fanyi.youdao.com/',
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.31 Safari/537.36'
    }
    url ='http://fanyi.youdao.com/translate?smartresult=dict&smartresult=rule'

    data={
        'i':content,
        'from':'AUTO',
        'to':'AUTO',
        'smartresult':'dict',
        'client':'fanyideskweb',
        'salt':'1539331760049',
        'sign':'b6fda24bcbc111b2dae5ba3889148316',
        'doctype':'json',
        'version':'2.1',
        'keyfrom':'fanyi.web',
        'action':'FY_BY_REALTIME',
        'typoResult':'false'
    }
    response = requests.post(url, headers=headers,data = data)
    if response.status_code ==200:
        items = response.json()
        item = items['translateResult'][0][0]
        # print(item.get('tgt'))
        var_translate_words.set(items['translateResult'][0][0]['tgt'])
    
btn1 = tk.Button(window,text='翻译',command = get_translate).place(x=75,y=145)
btn2 = tk.Button(window,text='退出',command=window.quit).place(x=175,y=145)

window.mainloop()
