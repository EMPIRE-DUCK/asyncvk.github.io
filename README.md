![vkbee](https://github.com/asyncvk/vkbee/blob/master/vkbee/bgtio.png?raw=true)
### Документация
## Вызов запросов к ВКидерасте
```python
import vkbee
import asyncio
loop = asyncio.get_event_loop()
vk_s = vkbee.VkApi('token', loop=loop)
async def main(loop):
  vk = vk_s.get_api()
  a = await vk.messages.send(
    chat_id=1,
    message='VKBEE TOP!',
    random_id=0
  )
  print(a)
asyncio.get_event_loop().run_until_complete(main(loop))
```
либо можно использовать ещё один асинхронный вариант вызова

```python
vk.async_call()
```
![Таблица](https://github.com/asyncvk/asyncvk.github.io/blob/master/KD8y1AGc3ds.jpg?raw=true)

## Параметры

| Параметр | Описание |
| -------- | ---------|
| vk_s | Ваш авторизованный аккаунт в модуле      |
| vk | Переменная для удобного вызова к API |
| messages.send | Метод ВКонтакте      |
| chat_id=1, ... | Аргументы параметров      |

## Подключение аккаунта

```python
import vkbee
import asyncio
loop = asyncio.get_event_loop()
vk_s = vkbee.VkApi('token', loop=loop)
vk = vk_s.get_api()
```
## Параметры

| Параметр | Описание |
| -------- | ---------|
| token | Ваш ключ доступа к аккаунту      |
| loop | Async Loop заданный  loop = asyncio.get_event_loop()     |

## Подсказка по парсингу ответа

```python
print((a['execute_errors'][0])
```

| Параметр | Описание |
| -------- | ---------|
| a | Сам запрос      |
| execute_errors | 1 подгруппа ответа храниться в array     |
| [0] | Для парсинга дальше либа возвращает массив с 0 идентификатором    |

Далее берем что хотим =)

### Подсказочка
   text и peer_id в примере сразу будут забиты,вам останеться только обработать их и ответить на них!

# Код без ошибки Unclosed Loop
## Эта ошибка из-за aiohttp, решается так
```python
import vkbee
import asyncio
vk = vkbee.VkApi('и снова седой токен',loop=asyncio.get_event_loop())
async def t():
	# Paste your code here
	
	await vk.s.close() # Paste this in stop
asyncio.run(t())
```

# BotsLongPoll
## Кода не будет токо пример
## Работает пока что только на 1.6
```bash
pip3 install vkbee==1.6
```

```python
import time, json, vkbee, asyncio, requests

async def main(loop):
    vk = vkbee.VkApi('опа почему токен не сполил',
        loop=loop,
    )
    vk_polluse = vkbee.BotLongpoll(vk, 'вставь ид группы', 10)
    async for event in vk_polluse.events():
        start = time.time()
        peer_id = event['object']['message']['peer_id']
        text = event['object']['message']['text'].lower()
        user_id = event['object']['message']['from_id']
        if text == '/help':
            await vkbee.VkApi.call(vk, 'messages.send', {'random_id': 0, 'peer_id': peer_id, 'message': 'Команды:\n/id - получение ID пользователя\n/say [текст] - повторяет текст\n/flip [текст] - переврнуть текст\n/weather [город] - получение погоды в указанном городе'})
        elif text == '/id':
            await vkbee.VkApi.call(vk, 'messages.send', {'random_id': 0, 'peer_id': peer_id, 'message': user_id})
        elif text.split()[0] == '/say':
            await vkbee.VkApi.call(vk, 'messages.send', {'random_id': 0, 'peer_id': peer_id, 'message': text.split(maxsplit=1)[1:]})
        elif text.split()[0] == '/flip':
            await vkbee.VkApi.call(vk, 'messages.send', {'random_id': 0, 'peer_id': peer_id, 'message': '&#38;#8238;' + str(text.split(maxsplit=1)[1:][0])})
        elif text.split()[0] == '/weather':
            city = text.split()[1]
            while True:
                try:
                    res = requests.get('http://cugadese.000webhostapp.com/weather?city=' + str(city))
                    data = res.json()
                    data=data['list'][0]
                    await vkbee.VkApi.call(vk, 'messages.send', {'random_id': 0, 'peer_id': peer_id, 'message': 'Город: ' + str(city) + '\nОблачность: ' + str(data['weather'][0]['description']) + '\nТемпература: ' +  str(data['main']['temp'])})
                    break
                except:
                    await vkbee.VkApi.call(vk, 'messages.send', {'random_id': 0, 'peer_id': peer_id, 'message': 'Не верное название города'})
                    break
        elif text == '/info':
            getinfo = {'user_ids': user_id}
            data = await vkbee.VkApi.call(vk, 'users.get', getinfo)
            stop = time.time()
            speed = stop - start
            keyboard = json.dumps(
                {
                    'one_time': False,
                    'buttons': [
                        [
                            {
                                'action': {
                                    'type': 'open_link',
                                    'link': 'https://asyncvk.github.io',
                                    'label': 'Открыть документацию VKBee',
                                }
                            }
                        ],
                        [
                            {
                                'action': {
                                    'type': 'text',
                                    'label': 'Скорость обработки события:',
                                },
                                'color': 'positive',
                            }
                        ],
                        [
                            {
                                'action': {'type': 'text', 'label': speed},
                                'color': 'negative',
                            }
                        ],
                    ],
                    'inline': True
                }
            )
            dast = {
                'random_id': 0,
                'peer_id': peer_id,
                'message': 'Информация о тебе: \nИмя: '
                + str(data['response'][0]['first_name'])
                + '\nФамилия: '
                + str(
                    data['response'][0]['last_name']
                    + '\nСписок всех моих команд: /help'
                ),
                'keyboard': keyboard,
            }
            await vkbee.VkApi.call(vk, 'messages.send', dast)

loop = asyncio.get_event_loop()
loop.run_until_complete(main(loop))
```


# UserLongPoll
## Подключение LongPoll для юзера

```python
  import vkbee
  from vkbee.longpoll import VkBeeLongpoll
  vk_s = vkbee.VkApi('ваштокен', loop=loop)
  vk = vk_s.get_api()
  longpoll = VkBeeLongpoll(vk_s)
```

## Слушаем ивенты от ВКонтакте

```python
  async for event in longpoll.events():
    peer_id = event.obj.peer_id
    text = event.obj.text
```

### Подсказочка
   text и peer_id в примере сразу будут забиты,вам останеться только обработать их и ответить на них!

## Ответ на ивент

```python
              if 'vkbee' in text:
                await vk.messages.send(
                  message='fastest!',
                  random_id=0,
                  chat_id=event.chat_id
                )
```
