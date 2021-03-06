# 1. Introduction
For this repo, I will collect and recover the censored (deleted) articles of WeChat Public Accounts and make them publicly available. If you want to make a new WeChat Public Account monitored by us, please do not hesitate to contact me through my E-mail below and give me the ID of the public account.  

该Repo用以收集和恢复被审查 (删除) 掉的微信公众号文章，并使其重见天日。如您希望将一个未被我们收录并监控的微信公众号登记在我们的项目里，请联系下方邮箱并提供该公众号的ID。  

# 2. Get Basic Variables
```python
re = requests.get('https://wechatscope.jmsc.hku.hk/api/update_weixin_public_pretty?days=7')
data = bs.BeautifulSoup(re.content, 'html.parser').text
data = data.replace('"','$$$').replace('{    $$$','{    "').replace('$$$: $$$','": "').\
replace('$$$,    $$$','",    "').replace('$$$  }','"  }').replace('$$$','')
data = json.loads(data)
df = pd.DataFrame(data)
df['archive'] = ['https://wechatscope.jmsc.hku.hk/api/html?fn='+i for i in df['archive']]
df['collecting_time']=collecting_time()
```

# 3. Get Original Articles
```python
num = 0
Content = []
for url in df['archive']:
    re = requests.get(url)
    soup = bs.BeautifulSoup(re.content, 'html.parser')
    for content in soup.find_all('a'):
        Content_=[]
        if '阅读全文' in str(content):
            for content in soup.find_all('a',id='js_share_source'):
                re=requests.get(content.get('href'))
                soup = bs.BeautifulSoup(re.content, 'html.parser')
                for content in soup.find_all('div',class_='rich_media_content'):
                    Content_.append(content.text)
        else:
            
            for content in soup.find_all('div',class_='rich_media_content'):
                Content_.append(content.text)
    Content.append(''.join(Content_))
    print(url+'\n↑ Article_'+str(num)+'......retrieved successfully\n')
    num = num + 1
```

# 4. Use the API and Data
To use the API and data, please follow the form of citation:  

Fu, K. W., & Yun, T. (2018). Tracking Censorship of WeChat Public Accounts in China: A Computational Social Science’s Approach, Presented at the 2018 International Conference on Computational Social Science,12-15 July 2018, Evanston, Illinois, United States.

While, people in Mainland China may not be able to get the access to this API due to the Great Wall. But just download the data and I'll keep updating. By the way, GitHub is a platform to make academic, scientific, and technological discussions. Please do not post any political view on my GitHub page though I am focusing on political communicaiton and public opinion. A right  and open discussion should be made on right context and platform. Thanks a lot!  

中国大陆网友恐无法直接接入该API下载数据，但可以自由取用我已下载好的数据，我将持续更新。顺带着说一句，GitHub是一个交流学术，科学，与技术的平台。请勿在本人GitHub主页上发表任何政治言论，虽然本人的研究方向是政治传播与公众舆论。一个合理且开放的讨论，应该在一个合适的平台上展开。非常感谢！

----
Xin Jin  
Senior Research Assistant  
Dept. Media and Communication, City Univ. HK  
[About](www.xjin.tech) | [LinkedIn](linkedin.com/in/xjin613/) | [Twitter](https://twitter.com/xjin_comm) | xin.jin@cityu.edu.hk
