from flask import Flask, request
import requests
import os

app = Flask(__name__)

BOT_TOKEN=os.environ.get("7986897418:AAE1k_5eQBVLpRLBwsRBp0f2MxzXqm-a_sY")
CHAT_ID = os.environ.get("7889676326")

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
