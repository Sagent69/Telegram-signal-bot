from flask import Flask, request
import requests
import os

app = Flask(__name__)

BOT_TOKEN = os.environ.get("BOT_TOKEN")
CHAT_ID = os.environ.get("CHAT_ID")

@app.route('/', methods=['POST'])
def webhook():
    data = request.json
    pair = data.get('pair')
    signal = data.get('signal')
    entry_time = data.get('entry_time')
    price = data.get('price')
    
    message = f"""
🔥 *Pocket Option Signal*
Pair: {pair}
Signal: {signal}
Entry Time: {entry_time}
Price: {price}
    """
    
    requests.post(f'https://api.telegram.org/bot{BOT_TOKEN}/sendMessage', data={
        'chat_id': CHAT_ID,
        'text': message,
        'parse_mode': 'Markdown'
    })

    return 'OK', 200
