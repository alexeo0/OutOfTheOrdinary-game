import telebot
import requests
import random
import ssl

ssl._create_default_https_context = ssl._create_unverified_context

token = 'сюда токен бота'

bot = telebot.TeleBot(token=token)

URL = "https://llm.api.cloud.yandex.net/foundationModels/v1/completion"

def run(iam_token, folder_id, user_text):
    global names, texts

    prompt = prompt = f"""
Ты участвуешь в игре "Победа, поражение и банан". У тебя карточка "Победа". 
Твоя цель - отгадать, кто из игроков получил карточку "Банан", анализируя их сообщения.

Правила:
1. Игрок с "Победой" должен вычислить "Банан"
2. Игрок с "Поражением" будет пытаться сбить тебя с толку
3. "Банан" может вести себя как угодно

Формат ввода: "<Имя игрока>#<сообщение>".
Имена игроков - {names[0]} и {names[1]}
Если ты хочешь обращатся в игрокам, то напиши им в таком формате: "Игрок <имя игрока>, <вопрос или обращение>", но после того как ты задала вопрос ход уже у другого игрока, так что задавай вопросы следующему игроку
НО каждый раз ход переходит к другому игроку.
Ты видишь историю сообщений всех игроков в переменной {texts}.

Анализируй сообщения игроков, их манеру общения и содержание. 
Если ты уверен, что определил "Банан", ответь в формате: "%<имя игрока>"

Твои ответы должны быть краткими, чтобы не раскрывать свою роль. 
Можешь задавать уточняющие вопросы или делать предположения, не называя "Банан", пока не уверен.
Игрока будут тебе врать и говорить неправду, но ты должна их раскусить

Текущий раунд. История сообщений:
{texts}

Твой ход (помни, что если ты уверен в ответе, нужно отправить %<имя игрока>)

Ещё напоследок дам тебе совет: чтобы выиграть прослушай игроков раз 5 хотя-бы
"""
    data = {
        "modelUri": f"gpt://{folder_id}/yandexgpt",
        "completionOptions": {"temperature": 0.3, "maxTokens": 1000},
        "messages": [
            {"role": "system", "text": prompt},
            {"role": "user", "text": f"{user_text}"},
        ]
    }

    texts[user_text.split('#')[0]].append(user_text.split('#')[1])

    response = requests.post(
        URL,
        headers={
            "Accept": "application/json",
            "Authorization": f"Api-key {iam_token}"
        },
        json=data,
    ).json()

    return response['result']['alternatives'][0]['message']['text']


@bot.message_handler(commands=['start_game'])
def start(message):
    global game
    game = True
    bot.send_message(message.chat.id, 'Введите имя первого игрока')

@bot.message_handler(commands=['stop_game'])
def stop(message):
    global game
    game = False
    bot.send_message(message.chat.id, 'Игра остановлена')

@bot.message_handler(content_types=['text'])
def f(message):
    global names, texts, answer, banana, game

    if not game:
        return
    
    if len(names) < 2:
        names.append(message.text)
        texts[names[len(names)-1]] = []
        if len(names) == 1:
            bot.reply_to(message, 'Введите имя второго игрока')
        elif len(names) == 2:
            banana = random.choice(names)
            bot.reply_to(message, f'Игра началась. В роли банана игрок {banana}. Очередь игрока {names[0]}')
            return
    else:
        answer = run(iam_token='AQVNxOfWqiNd654V39p-QjdiR7IczE4PTxp1wm4Z', folder_id='b1g7p0kad01qi681gqps', user_text=names[0]+'#'+message.text)
        if answer[0] == '%':
            bot.send_message(message.chat.id, f'Я думаю игрок {answer[2:]} это банан')
            if answer[2:] == banana:
                bot.send_message(message.chat.id, f'SYSTEM:\nНейросеть угадала! Победил игрок {banana}')
                game = False
                return
            else:
                bot.send_message(message.chat.id, f'SYSTEM:\nНейросеть не угадала! Победил игрок {answer[2:]}')
                game = False
                return
        else:
            bot.reply_to(message, answer)

names = []
banana = ''
hod = '' # -1 - 0, 1-1
texts = {}
answer = ''
game = None

bot.polling(none_stop=True)
