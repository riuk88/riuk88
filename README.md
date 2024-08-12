import telebot
import requests
import json
import datetime

bot = telebot.TeleBot('6673217588:AAGiFT7uJaHwHhM31-yMh3TSAkI0SAS9RhY')
API = '80daaf9a11f0926bbc3826ec426320a7'

@bot.message_handler(commands=['start'])
def start(message):
    bot.send_message(message.chat.id, 'Перивет, рад тебя видеть! Напиши названеи города')


@bot.message_handler(content_types=['text'])
def get_weather(message):
    city = message.text.strip().lower()
    res =requests.get(f'https://api.openweathermap.org/data/2.5/weather?q={city}&appid={API}&units=metric&ru&callback')
    if res.status_code ==200:
        data = json.loads(res.text)
        bot.reply_to(message,
                     f'Погода на данный момент\n'
                     f'В градусах Цельсия в С°: {data["main"]["temp"]}\n'
                     f'Влажность в %: {data["main"]["humidity"]}\n'
                     f'Скорость ветсра в м/с: {data["wind"]["speed"]}\n'
                     f'Облачность в %: {data["clouds"]["all"]}\n'
                     )




    else:
        bot.reply_to(message,'Город указан не верно')

bot.polling(none_stop=True)
