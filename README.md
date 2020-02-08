![vkbee](https://github.com/asyncvk/vkbee/blob/master/vkbee/bgtio.png?raw=true)
### Документация
## Вызов запросов к ВКидерасте
```python
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
либо можно использовать синхронный вариант вызова

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
| [0] | Для   парсинга дальше либа возвращает массив с 0 идентификатором    |

Далее берем что хотим =)

### Подсказочка
   text и peer_id в примере сразу будут забиты,вам останеться только обработать их и ответить на них!

## Ответ на ивент

```python
              if 'vkbee' in text:
                dast = {
                    'random_id': 0,
                    'peer_id': peer_id,
                    'message': 'Класс супер бомба!!!!!!'
                }
                await vk_s.call('messages.send', dast)
```

### Подсказочка 
   random_id всегда нужен,иначе ВКонтакте не обработает ваш запрос!

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
