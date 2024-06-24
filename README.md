import requests
from bs4 import BeautifulSoup
#
#
# # Функция для получения новостей с веб-страницы
def get_news(url):
 response = requests.get(url)
soup = BeautifulSoup(response.content, 'html.parser')
#
#     # Находим все заголовки новостей
titles = soup.find_all('h2', class_='news-title')
#
#     # Проходимся по каждому заголовку и извлекаем нужные данные
for title in titles:
#         # Получаем заголовок
         news_title = title.text.strip()
#
#         # Получаем аннотацию
         news_summary = title.find_next('p').text.strip()
#
#         # Получаем авторов
         authors = title.find_next('span', class_='author').text.strip()
#
#         # Проверяем, содержит ли новость упоминания представителей России и США
         if 'Байден' in news_title or 'Путин' in news_title or 'конфликт' in news_title:
             print('Заголовок:', news_title)
#            print('Аннотация:', news_summary)
             print('Авторы:', authors)
             print('---')
#
#
# # Основная функция для запуска скрипта на определенное время
 def run_script(duration):
#     # Задаем URL страницы новостного агентства
     url = 'https://www.kommersant.ru/'
#
#     # Запускаем цикл на заданное время (в данном примере - 4 часа)
#     # Можно использовать другие способы для контроля времени (например, модуль time)
     for _ in range(duration):
         get_news(url)

#
# # Запуск скрипта на 4 часа
 run_script (14400)
import requests
from bs4 import BeautifulSoup
import time
from datetime import datetime, timedelta


# Функция для извлечения новостей
def fetch_news(url):
    try:
        response = requests.get(url)
        soup = BeautifulSoup(response.text, 'html.parser')

        # Найти элементы с новостями (зависит от структуры веб-страницы)
        news_items = soup.find_all('div', class_='news_item')  # Примерный класс

        for item in news_items:
            title = item.find('h2', class_='title').text.strip()  # Примерный класс
            summary = item.find('p', class_='summary').text.strip()  # Примерный класс
            authors = item.find('span', class_='authors').text.strip()  # Примерный класс

            # Проверка на упоминание партий
            if 'Путин' in item.text or 'Байден' in item.text or 'конфликт' in item.text:
                print(f'Заголовок: {title}')
                print(f'Аннотация: {summary}')
                print(f'Авторы: {authors}')
                print('---')
            else:
                print('Пока ничего не найдено')
    except Exception as e:
        print(f'Ошибка при извлечении новостей: {e}')


# Основная функция для запуска скрипта на определенное время
def run_script(duration_hours):
    end_time = datetime.now() + timedelta(hours=duration_hours)
    print(f'Скрипт запущен на {duration_hours} часов. Время окончания: {end_time}')

    while datetime.now() < end_time:
        # URL новостного агентства
        url = 'https://www.kommersant.ru/'
        fetch_news(url)

        # Пауза перед следующим запросом
        time.sleep(900)
        print('Новый запуск поиска')


# Запуск скрипта на 4 часа
run_script(14400)

