# My-Firstapp
from flask import Flask, render_template, request, jsonify
from twilio.rest import Client

app = Flask(__name__)

account_sid = 'your_twilio_account_sid'
auth_token = 'your_twilio_auth_token'
client = Client(account_sid, auth_token)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/chat', methods=['POST'])
def chat():
    user_message = request.form['message']
    response_message = f"You said: {user_message}"
    return jsonify({"response": response_message})

@app.route('/send_sms', methods=['POST'])
def send_sms():
    to_number = request.form['to']
    message_body = request.form['message']
    message = client.messages.create(
        to=to_number,
        from_="your_twilio_phone_number",
        body=message_body
    )
    return jsonify({"status": "Message sent", "sid": message.sid})

@app.route('/dashboard')
def dashboard():
    return render_template('dashboard.html')

@app.route('/submit_question', methods=['POST'])
def submit_question():
    question = request.form['question']
    return jsonify({"response": f"Your question: '{question}' has been received."})

if __name__ == '__main__':
    app.run(debug=True)
    
