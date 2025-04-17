# Vosk TTS Home Assistant
Сервер и интеграция для [vosk-tts](https://github.com/alphacep/vosk-tts).
Создано Grok и Gemini. Понятие не имею как оно работает, сколько неоптимальных решений и лишнего кода используется.

Запуск сервера
```
mkdir vosk-tts-server
cd vosk-tts-server
# Скопировать main.py и err.wav (заглушка для HA, которая срабатывает при превышение MAX_TEXT_LENGTH,
# число символов подбирайте под производительность cpu, генерация должна быть короче 5с )
# Создайте и активируйте виртуальное окружение (рекомендуется)
python -m venv venv
source venv/bin/activate  # Linux/macOS
# venv\Scripts\activate  # Windows

# Установите необходимые библиотеки
pip install vosk-tts fastapi uvicorn[standard] soundfile python-dotenv numpy

uvicorn main:app --host 0.0.0.0 --port 5002 --reload

# Тест
http://127.0.0.1:5002/synthesize?text=Привет%2C%20мир
```


Интеграция

Скопировать каталог vosk_tts в /homeassistant/custom_components
В конфигурационном файле добавить запись, не забудьте указать верный адрес сервера:
```
tts:
  - platform: vosk_tts
    url: "http://192.168.1.xxx:5002/synthesize"
    default_voice: "4"
```
Перезапустить HA.

Можно использовать действие с выбором одного из пяти голосов (0-4):
```
action: tts.vosk_tts_say
data:
  entity_id: media_player.lg
  message: Привет 
  options:
    voice: "2"
```

Дорабатывайте и улучшайте на свое усмотретние.
