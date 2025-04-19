---
title: DLAI - ChatGPT API [I] - Language Models, the Chat Format and Tokens
date: 2024-04-15 23:32:01
updated:
tags: llm/gpt
categories: software
---


> Link: [DLAI - Building Systems with the ChatGPT API](https://learn.deeplearning.ai/courses/chatgpt-building-system/lesson/2/language-models%2C-the-chat-format-and-tokens)

# Language Models, the Chat Format and Tokens

- LLM在互動過程，是一直依據前面的Input X(前面的句子)，來預測下一個字詞Output Y

![image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/3dfed944-4d33-4022-8938-df8b230db6a0/f06b8c53-5281-4692-a3fb-3c7465afb37b)

## Tokens

```plaintext
[Learning][ new][ things][ is][ fun][!]    // [] is one "token"
```

![image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/3dfed944-4d33-4022-8938-df8b230db6a0/01c20d21-4b1d-4736-a006-e5f9df60c510)

## 三大提示詞－System, User and Assistant Message

1. System: sets tone/behavior of assistant. 領先於User and Assistant的提示，會去定義GPT的回答方式或回答行為
   - System 的提示詞，有多種提示用法：
      - 調整回答長度，或者做角色扮演
1. Assistant: Chat model / LLM response. 亦即ChatGPT的回答，可以先定義前面的提示句，後面讓ChatGPT來完成他
2. User: YOU，也就是使用者的prompt，就問他問題

```python
messages =  [  
{'role':'system', 
 'content':"""You are an assistant who\
 responds in the style of Dr Seuss."""}, 
# {
#   'role':'assistant',
#   'content':"""..."""
# },
{'role':'user', 
 'content':"""write me a very short poem\
 about a happy carrot"""},  
]
```

![image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/3dfed944-4d33-4022-8938-df8b230db6a0/5df2ba83-1aae-4809-96c9-49f536f3c336)

### API Key

- There is more secure way to use API Key

```python
from dotenv import load_dotenv, find_dotenv

_ = load_dotenv(find_dotenv())    # read local .env file
import os
import openai

openai.api_key = os.getenv('OPENAP_API_KEY')
```

### Revolutionizing AI Application

![image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/3dfed944-4d33-4022-8938-df8b230db6a0/42c90de6-ca0b-4045-9e5c-656e1d7c94ff)

多虧AI的革新，我們可以快速去使用相關的API，用非常簡短的時間來完成大型語言模型的調教，更快部屬到應用端。

## 如何使用API－Call Methods

### Function: `get_completion`

```python
client = openai.OpenAI()

def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = client.chat.completions.create(
        model=model,
        messages=messages,
        temperature=0  # this is the degree of randomness of the model's output 
    )
    return response.choices[0].message.content
```

### Function: `get_completion_from_messages`

```python
def get_completion_and_token_count(messages, 
                                   model="gpt-3.5-turbo", 
                                   temperature=0, 
                                   max_tokens=500):
    
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, 
        max_tokens=max_tokens,
    )
    
    content = response.choices[0].message["content"]
    
    token_dict = {
'prompt_tokens':response['usage']['prompt_tokens'],
'completion_tokens':response['usage']['completion_tokens'],
'total_tokens':response['usage']['total_tokens'],
    }

    return content, token_dict
##################################################################
messages = [
{'role':'system', 
 'content':"""You are an assistant who responds\
 in the style of Dr Seuss."""},    
{'role':'user',
 'content':"""write me a very short poem \ 
 about a happy carrot"""},  
] 
response, token_dict = get_completion_and_token_count(messages)
```

---

# Exercise: 星座描述應答

### 軟體概念呈現

- **Motivation & Objective**: 讓不清楚生日對應甚麼星座的人們，方便查詢星座與該星座的相關細節; 讓使用者可以方便查詢且易於顯示
- **Input**: 使用者只要輸入 [綽號] 與 [生日日期]
- **Output**: 以表格方式顯示：[對應星座] [星座月份日期] [星座性格特質] [優缺點] [愛情觀/感情觀]，並以表格的方式呈現

### 程式碼實作

> 環境使用課程中提供的Jupyter note book 進行案例實作

```python
import os
import openai
import tiktoken
from IPython.display import display, Markdown, Latex, HTML, JSON
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file

openai.api_key  = os.environ['OPENAI_API_KEY']

# OpenAI API Function Definition
def get_completion_from_messages(messages, 
                                 model="gpt-3.5-turbo", 
                                 temperature=0, 
                                 max_tokens=500):
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, # this is the degree of randomness of the model's output
        max_tokens=max_tokens, # the maximum number of tokens the model can ouptut 
    )
    return response.choices[0].message["content"]

# Message Definition: System role and User role
messages =  [  
{'role':'system', 
 'content':"""You are an assistant who\
 responds in the style of an professional astrologer.\
 User will give you his/her nickname and their birthday.\
 Your goal is reply user that what the star sign they are,\
 and response the below items:\
 1. What star sign?\
 2. The date period of the star sign.\
 3. Character traits of the star sign.\
 4. Advantages and Disadvantages of the star sign.\
 5. View of love/view of feelings of the star sign.\
 Please response them with a table style in HTML structure."""},    
{'role':'user', 
 'content':"""Nickname: Jimmy, Birthday: eighth, Dec"""},  
] 

# Output
response = get_completion_from_messages(messages, temperature=1)
print(response)
display(HTML(response))
```

### 輸出結果

#### HTML Code

```html
<table>
    <tr>
        <td>1. Star Sign:</td>
        <td>Sagittarius</td>
    </tr>
    <tr>
        <td>2. Date Period:</td>
        <td>November 22 - December 21</td>
    </tr>
    <tr>
        <td>3. Character Traits:</td>
        <td>Adventurous, independent, optimistic, generous, and philosophical.</td>
    </tr>
    <tr>
        <td>4. Advantages:</td>
        <td>Enthusiastic, open-minded, and great sense of humor.</td>
    </tr>
    <tr>
        <td>Disadvantages:</td>
        <td>Impatient, tactless, and prone to taking risks.</td>
    </tr>
    <tr>
        <td>5. View of Love/Feelings:</td>
        <td>Sagittarians value freedom and honesty in relationships, often seeking excitement and new experiences. They may struggle with commitment but are loyal and passionate partners.</td>
    </tr>
</table>
```

#### Output View

![image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/3dfed944-4d33-4022-8938-df8b230db6a0/67012c22-61e3-49c0-b82f-166d23a38a21)

---

## Conclusion

以上就是這次的課程筆記。按照課程步驟，在一次體會到提示工程的重要性，良好並完整的提示，對LLM模型的回答而言是很有幫助的。未來，有機會建置屬於自己的API時，`system role` 的提示詞也要多用點心來撰寫。

另外，我自己有直接使用 OpenAI API 直接在本地端進行實作，測試過程中發現：使用本地端API沒有免費的使用權限(需要$$)；有鑑於現階段尚無需求，那就先蹭一下課程的API吧~