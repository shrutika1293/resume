# Step 1 - install ngrok
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null
echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list
sudo apt update && sudo apt install ngrok -y

# Step 2 - sign up at ngrok.com and get your authtoken
ngrok config add-authtoken your_token_here

# Step 3 - start InfraChat
cd /home/shrutika/InfraChat
docker compose up -d

# Step 4 - expose it publicly
ngrok http 80
