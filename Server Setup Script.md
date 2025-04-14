# Server Setup Script

This script sets up a basic Python Flask server on a Linux machine.

```bash
#!/bin/bash

echo " Starting server setup..."

# Update & upgrade
sudo apt update && sudo apt upgrade -y

# Install Python and pip
sudo apt install python3 python3-pip -y

# Install Flask
pip3 install Flask

# Create app directory
mkdir -p ~/app
cd ~/app

# Create basic Flask app
cat <<EOL > app.py
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from automated setup!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=8080)
EOL

# Open firewall port 8080
sudo ufw allow 8080

# Run the app
echo "✅ Setup complete! Starting Flask app on port 8080..."
python3 app.py

```
