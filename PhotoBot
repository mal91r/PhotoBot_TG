import telebot
from telebot import types
from PIL import Image, ImageDraw
from urllib.request import urlopen
import numpy

import cv2


token = '1902011112:AAFaySWil0icuhsgAO-rxr7tcN8jqWubfYE'
bot = telebot.TeleBot(token)

@bot.message_handler(content_types=['text'])
def welcome_message(message):

    if message.text == '/start':

        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        item1 = types.KeyboardButton("Фильтр")
        item2 = types.KeyboardButton("Улучшение")
        markup.add(item1, item2)

        bot.send_message(message.from_user.id, "Привет! Я - бот по обработке фотографий. Для начала работы выберите одну из функций: наложить фильтр или улучший один из параметров!",reply_markup=markup)
        bot.register_next_step_handler(message, request)

    else:

        bot.send_message(message.from_user.id, "Для начала напишите /start")

def request(message):
    if message.text == 'Фильтр':
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        item1 = types.KeyboardButton("Black&White")
        item2 = types.KeyboardButton("Negative")
        item3 = types.KeyboardButton("Sepia")
        markup.add(item1, item2, item3)
        bot.send_message(message.chat.id, "Отлично! Теперь выберите фильтр, который необходимо наложить!", reply_markup=markup)
        bot.register_next_step_handler(message, filter)
    elif message.text == 'Улучшение':
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        item1 = types.KeyboardButton("Size")
        item2 = types.KeyboardButton("Gamma")
        markup.add(item1, item2)
        bot.send_message(message.chat.id, "Отлично! Теперь выберите параметр, который необходимо изменить!", reply_markup=markup)
        bot.register_next_step_handler(message, quality)
    else:
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        item1 = types.KeyboardButton("Фильтр")
        item2 = types.KeyboardButton("Улучшение")
        markup.add(item1, item2)
        bot.send_message(message.chat.id, "Ошибка! Неверное название функции..", reply_markup=markup)
        bot.register_next_step_handler(message, request)

def quality(message):

    if message.text == 'Size':

        bot.send_message(message.chat.id, "Отлично! Теперь скиньте фото, которое нужно обработать!")
        bot.register_next_step_handler(message, Size)
    elif message.text == 'Gamma':
        bot.send_message(message.chat.id, "Отлично! Теперь скиньте фото, которое нужно обработать!")
        bot.register_next_step_handler(message, gamma)

    else:
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        item1 = types.KeyboardButton("Size")
        item2 = types.KeyboardButton("Gamma")
        markup.add(item1, item2)
        bot.send_message(message.chat.id, "Неверно указано название операции, попробуйте снова..", reply_markup=markup)
        bot.register_next_step_handler(message, quality)

def filter(message):

    if message.text == 'Black&White':

        bot.send_message(message.chat.id, "Отлично! Теперь скиньте фото, которое нужно обработать!")
        bot.register_next_step_handler(message, black_white)

    elif message.text == 'Negative':

        bot.send_message(message.chat.id, "Отлично! Теперь скиньте фото, которое нужно обработать!")
        bot.register_next_step_handler(message, negative)

    elif message.text == 'Sepia':

        bot.send_message(message.chat.id, "Отлично! Теперь скиньте фото, которое нужно обработать!")
        bot.register_next_step_handler(message, sepia)

    else:

        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        item1 = types.KeyboardButton("Black&White")
        item2 = types.KeyboardButton("Negative")
        item3 = types.KeyboardButton("Sepia")
        markup.add(item1, item2, item3)

        bot.reply_to(message, "Неверно указано название операции, попробуйте снова..", reply_markup=markup)
        bot.register_next_step_handler(message, filter)

def gamma(message):
    if message.content_type == 'photo':
        idphoto = message.photo[len(message.photo)-1].file_id
        photo_info = bot.get_file(idphoto)
        link = f'http://api.telegram.org/file/bot{token}/{photo_info.file_path}'
        img = Image.open(urlopen(link))
        img = img.save("ans.png")
        im = cv2.imread("ans.png")
        im = im/255.0
        im_power_law_transformation = cv2.pow(im,1.5)
        result = cv2.imwrite("rez.png", (im_power_law_transformation*255).astype(numpy.uint8))
        img = Image.open("rez.png")
        bot.send_photo(message.chat.id, img)
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        item1 = types.KeyboardButton("/start")
        markup.add(item1)
        bot.send_message(message.chat.id, "Обработка завершена! Для начала новой нажмите /start", reply_markup=markup)
    else:
        bot.send_message(message.chat.id, "Неверный тип входного файла, отправьте, пожалуйста, фотографию..")
        bot.register_next_step_handler(message, gamma)

def Size(message):
    if message.content_type == 'photo':
        idphoto = message.photo[len(message.photo)-1].file_id
        photo_info=bot.get_file(idphoto)
        link = f'http://api.telegram.org/file/bot{token}/{photo_info.file_path}'
        img = Image.open(urlopen(link))
        draw = ImageDraw.Draw(img)
        width = img.size[0]
        height = img.size[1]
        pix = img.load()
        new_img = img.resize((2*width, 2*height))
        bot.send_photo(message.chat.id, new_img)
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        item1 = types.KeyboardButton("/start")
        markup.add(item1)
        bot.send_message(message.chat.id, "Обработка завершена! Для начала новой нажмите /start", reply_markup=markup)
    else:
        bot.send_message(message.chat.id, "Неверный тип входного файла, отправьте, пожалуйста, фотографию..")
        bot.register_next_step_handler(message, Size)
def black_white(message):
    if message.content_type == 'photo':
        idphoto = message.photo[len(message.photo)-1].file_id
        photo_info=bot.get_file(idphoto)
        link = f'http://api.telegram.org/file/bot{token}/{photo_info.file_path}'
        img = Image.open(urlopen(link))
        draw = ImageDraw.Draw(img)
        width = img.size[0]
        height = img.size[1]
        pix = img.load()
        for i in range(width):
            for j in range(height):
                a=pix[i,j][0]
                b=pix[i,j][1]
                c=pix[i,j][2]
                s=(a+b+c)//3
                draw.point((i,j),(s,s,s))
        del draw
        bot.send_photo(message.chat.id, img)
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        item1 = types.KeyboardButton("/start")
        markup.add(item1)
        bot.send_message(message.chat.id, "Обработка завершена! Для начала новой нажмите /start", reply_markup=markup)
    else:
        bot.send_message(message.chat.id, "Неверный тип входного файла, отправьте, пожалуйста, фотографию..")
        bot.register_next_step_handler(message, quality)

def negative(message):
    if message.content_type == 'photo':
        idphoto = message.photo[len(message.photo)-1].file_id
        photo_info=bot.get_file(idphoto)
        link = f'http://api.telegram.org/file/bot{token}/{photo_info.file_path}'
        img = Image.open(urlopen(link))
        draw = ImageDraw.Draw(img)
        width = img.size[0]
        height = img.size[1]
        pix = img.load()
        for i in range(width):
            for j in range(height):
                a = pix[i, j][0]
                b = pix[i, j][1]
                c = pix[i, j][2]
                draw.point((i, j), (255 - a, 255 - b, 255 - c))
        del draw
        bot.send_photo(message.chat.id, img)
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        item1 = types.KeyboardButton("/start")
        markup.add(item1)
        bot.send_message(message.chat.id, "Обработка завершена! Для начала новой нажмите /start", reply_markup=markup)
    else:
        bot.send_message(message.chat.id, "Неверный тип входного файла, отправьте, пожалуйста, фотографию..")
        bot.register_next_step_handler(message, quality)

def sepia(message):
    if message.content_type == 'photo':
        idphoto = message.photo[len(message.photo)-1].file_id
        photo_info=bot.get_file(idphoto)
        link = f'http://api.telegram.org/file/bot{token}/{photo_info.file_path}'
        img = Image.open(urlopen(link))
        draw = ImageDraw.Draw(img)
        width = img.size[0]
        height = img.size[1]
        pix = img.load()
        depth = 30
        for i in range(width):
            for j in range(height):
                a = pix[i, j][0]
                b = pix[i, j][1]
                c = pix[i, j][2]
                S = (a + b + c) // 3
                a = S + depth * 2
                b = S + depth
                c = S
                if (a > 255):
                    a = 255
                if (b > 255):
                    b = 255
                if (c > 255):
                    c = 255
                draw.point((i, j), (a, b, c))
        del draw
        bot.send_photo(message.chat.id, img)
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        item1 = types.KeyboardButton("/start")
        markup.add(item1)
        bot.send_message(message.chat.id, "Обработка завершена! Для начала новой нажмите /start", reply_markup=markup)
    else:
        bot.send_message(message.chat.id, "Неверный тип входного файла, отправьте, пожалуйста, фотографию..")
        bot.register_next_step_handler(message, quality)

bot.polling(none_stop=True)
