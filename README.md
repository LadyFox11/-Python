# -Python
Бота на мові Python, який показує погоду у Лос-Анджелесі. Для цього ми можемо скористатися API сервісу OpenWeatherMap.
код, який відповідає на повідомлення /weather в Telegram та надсилає інформацію про погоду в Лос-Анджелесі:
import telegram
import pyowm

# ініціалізація бота та API OpenWeatherMap
TOKEN = 'YOUR_TOKEN_HERE'
bot = telegram.Bot(token=TOKEN)
owm = pyowm.OWM('YOUR_API_KEY_HERE')

# функція відправки повідомлення
def send_message(chat_id, text):
    bot.send_message(chat_id=chat_id, text=text)

# функція обробки команди /weather
def weather(update, context):
    try:
        # отримання інформації про погоду
        observation = owm.weather_at_place('Los Angeles, US')
        weather = observation.get_weather()
        temperature = weather.get_temperature(unit='celsius')['temp']
        status = weather.get_detailed_status()

        # формування повідомлення
        text = f"Погода в Лос-Анджелесі:\nТемпература: {temperature}°C\nСтатус: {status}"
        send_message(update.message.chat_id, text)
    except Exception as e:
        send_message(update.message.chat_id, f"Помилка: {e}")

# створення обробників
from telegram.ext import Updater, CommandHandler

updater = Updater(TOKEN, use_context=True)
dispatcher = updater.dispatcher
dispatcher.add_handler(CommandHandler('weather', weather))

# запуск бота
updater.start_polling()
updater.idle()



Цей код створює обробник команди /weather, який використовує API OpenWeatherMap для отримання інформації про погоду в Лос-Анджелесі. Якщо все пройде успішно, бот відправляє повідомлення з інформацією про температуру та статус погоди. Якщо виникає помилка, бот повідомляє про це користувача.

Перед запуском цього коду, треба встановити бібліотеки telegram та pyowm за допомогою pip.


Після того, як ви встановили бібліотеки telegram та pyowm за допомогою pip, вам необхідно внести кілька змін у код.

Замініть YOUR_TOKEN_HERE на свій токен бота Telegram. Щоб отримати токен, відкрийте @BotFather у Telegram та створіть нового бота.

Замініть YOUR_API_KEY_HERE на свій API ключ OpenWeatherMap. Щоб отримати ключ, зареєструйтеся на сайті OpenWeatherMap та створіть новий API ключ.

Після цього збережіть файл як weather_bot.py та запустіть його в терміналі за допомогою команди python weather_bot.py. Якщо все пройде успішно, бот буде запущений і готовий відповідати на команду /weather в Telegram.
