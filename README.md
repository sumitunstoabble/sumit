# sumit
from flask import Flask, jsonify, request

app = Flask(__name__)

# In-memory database simulation
users = {
    1: {'name': 'John Doe', 'email': 'john@example.com'},
    2: {'name': 'Jane Doe', 'email': 'jane@example.com'}
}

@app.route('/')
def welcome():
    return jsonify({'message': 'Welcome to the API!'})

@app.route('/user/<int:user_id>', methods=['GET'])
def get_user(user_id):
    user = users.get(user_id)
    if user:
        return jsonify(user)
    else:
        return jsonify({'error': 'User not found'}), 404

@app.route('/user', methods=['POST'])
def create_user():
    data = request.get_json()
    if 'name' not in data or 'email' not in data:
        return jsonify({'error': 'Invalid data'}), 400
    
    new_id = max(users.keys()) + 1
    users[new_id] = {'name': data['name'], 'email': data['email']}
    return jsonify({'message': 'User created', 'user': users[new_id]}), 201

if __name__ == '__main__':
    app.run(debug=True)
