import telebot
import requests
import time
from bs4 import BeautifulSoup

token = "////////////////////////"
channel_id = "@newscriptosdrops"
bot = telebot.TeleBot(token)

@bot.message_handler(content_types=['text'])
def commands(message):
    # bot.send_message(channel_id, message.text)
    if message.text == "Старт":
        # bot.send_message(channel_id, "Hello")
        back_post_id = None
        while True:
            post_text = parser(back_post_id)
            back_post_id = post_text[1]
            
            if post_text[0] != None:
                bot.send_message(channel_id, post_text[0])
                time.sleep(300)
            else:
                bot.send_message(message.from_user.id, "Я тебя не понимаю. Напиши Старт")

def parser(back_post_id):
    URL = "https://forklog.com/news"
    page = requests.get(URL)
    soup = BeautifulSoup(page.content, "html.parser")
    
    post_text = parser(post_text[1])
    
    if requests.post['data-id'] != back_post_id:
        title = requests.post.find("a", class_="post__title_link").text.strip()
        description = requests.post.find("div", class_="post__text post__text-html post__text_v1").text.strip()
        url = requests.post.find("a", class_="post__title_link", href=True)["href"].strip()
        return f"{title}\n\n{description}\n\n{url}", requests.post['data-id']
    else:
        return None, requests.post['data-id']
