![vkbee](https://github.com/asyncvk/vkbee/blob/master/vkbee/bgtio.png?raw=true)
### Документация
## Вызов запросов к ВКидерасте
```python
import asyncio
loop = asyncio.get_event_loop()
vk = vkbee.VkApi('token', loop=loop)
async def main(loop):
  a=await vkbee.VkApi.call(vk,'status.set',{'status':'VKBee is fastest!','group_id':1})
  print(a)
asyncio.get_event_loop().run_until_complete(main(loop))
```
либо можно использовать синхронный вариант вызова

```python
vk.call()
```

## Параметры

| Параметр | Описание |
| -------- | ---------|
| vk | Ваш авторизованный аккаунт в модуле      |
| status.set | Метод ВКонтакте      |
| {'status':'VKBee is fastest!','group_id':1} | Json объект параметров      |

## Подключение аккаунта

```python
import asyncio
loop = asyncio.get_event_loop()
vk = vkbee.VkApi('token', loop=loop)
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

# BotsLongPoll
## Подключение LongPoll для группы

```python
  vk = vkbee.VkApi('ваштокен', loop=loop)
  vk_poll = vkbee.BotLongpoll(vk, "191502455", 10)
```

## Слушаем ивенты от ВКонтакте

```python
  async for event in vk_poll.events():
    peer_id = event['object']['message']['peer_id']
    text = event['object']['message']['text']
```

### Подсказочка
   text и peer_id в примере сразу будут забиты,вам останеться только обработать их и ответить на них!

## Ответ на ивент

```python
              if 'vkbee' in text.split():
                dast = {
                    'random_id': 0,
                    'peer_id': peer_id,
                    'message': 'Класс супер бомба!!!!!!'
                }
                await vkbee.VkApi.call(vk, 'messages.send', dast)
```

### Подсказочка 
   random_id всегда нужен,иначе ВКонтакте не обработает ваш запрос!

# UserLongPoll
## Подключение LongPoll для юзера

```python
  vk = vkbee.VkApi('ваштокен', loop=loop)
  vk_polluse = vkbee.BotLongpoll(vk,10)
```

## Слушаем ивенты от ВКонтакте

```python
  async for event in vk_polluse.events():
    peer_id = event['object']['message']['peer_id']
    text = event['object']['message']['text']
```

### Подсказочка
   text и peer_id в примере сразу будут забиты,вам останеться только обработать их и ответить на них!

## Ответ на ивент

```python
              if 'vkbee' in text.split():
                dast = {
                    'random_id': 0,
                    'peer_id': peer_id,
                    'message': 'Класс супер бомба!!!!!!'
                }
                await vkbee.VkApi.call(vk, 'messages.send', dast)
```

### Подсказочка 
   random_id всегда нужен от юзера,иначе ВКонтакте просто не разрешит отправить сообщение!
