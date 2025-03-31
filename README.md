![Banner image](https://user-images.githubusercontent.com/10284570/173569848-c624317f-42b1-45a6-ab09-f0ea3c247648.png)

# n8n - Secure Workflow Automation for Technical Teams

n8n is a workflow automation platform that gives technical teams the flexibility of code with the speed of no-code. With 400+ integrations, native AI capabilities, and a fair-code license, n8n lets you build powerful automations while maintaining full control over your data and deployments.

![n8n.io - Screenshot](https://raw.githubusercontent.com/n8n-io/n8n/master/assets/n8n-screenshot-readme.png)

## Key Capabilities

- **Code When You Need It**: Write JavaScript/Python, add npm packages, or use the visual interface
- **AI-Native Platform**: Build AI agent workflows based on LangChain with your own data and models
- **Full Control**: Self-host with our fair-code license or use our [cloud offering](https://app.n8n.cloud/login)
- **Enterprise-Ready**: Advanced permissions, SSO, and air-gapped deployments
- **Active Community**: 400+ integrations and 900+ ready-to-use [templates](https://n8n.io/workflows)

## Quick Start

Try n8n instantly with [npx](https://docs.n8n.io/hosting/installation/npm/) (requires [Node.js](https://nodejs.org/en/)):

```
npx n8n
```

Or deploy with [Docker](https://docs.n8n.io/hosting/installation/docker/):

```
docker volume create n8n_data
docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

Access the editor at http://localhost:5678

## Resources

- 📚 [Documentation](https://docs.n8n.io)
- 🔧 [400+ Integrations](https://n8n.io/integrations)
- 💡 [Example Workflows](https://n8n.io/workflows)
- 🤖 [AI & LangChain Guide](https://docs.n8n.io/langchain/)
- 👥 [Community Forum](https://community.n8n.io)
- 📖 [Community Tutorials](https://community.n8n.io/c/tutorials/28)

## Support

Need help? Our community forum is the place to get support and connect with other users:
[community.n8n.io](https://community.n8n.io)

## License

n8n is [fair-code](https://faircode.io) distributed under the [Sustainable Use License](https://github.com/n8n-io/n8n/blob/master/LICENSE.md) and [n8n Enterprise License](https://github.com/n8n-io/n8n/blob/master/LICENSE_EE.md).

- **Source Available**: Always visible source code
- **Self-Hostable**: Deploy anywhere
- **Extensible**: Add your own nodes and functionality

[Enterprise licenses](mailto:license@n8n.io) available for additional features and support.

Additional information about the license model can be found in the [docs](https://docs.n8n.io/reference/license/).

## Contributing

Found a bug 🐛 or have a feature idea ✨? Check our [Contributing Guide](https://github.com/n8n-io/n8n/blob/master/CONTRIBUTING.md) to get started.

## Join the Team

Want to shape the future of automation? Check out our [job posts](https://n8n.io/careers) and join our team!

## What does n8n mean?

**Short answer:** It means "nodemation" and is pronounced as n-eight-n.

**Long answer:** "I get that question quite often (more often than I expected) so I decided it is probably best to answer it here. While looking for a good name for the project with a free domain I realized very quickly that all the good ones I could think of were already taken. So, in the end, I chose nodemation. 'node-' in the sense that it uses a Node-View and that it uses Node.js and '-mation' for 'automation' which is what the project is supposed to help with. However, I did not like how long the name was and I could not imagine writing something that long every time in the CLI. That is when I then ended up on 'n8n'." - **Jan Oberhauser, Founder and CEO, n8n.io**


# ⚡ n8n Installing Guide with Docker & Portainer

Welcome to our comprehensive guide on installing **n8n** — an extendable workflow automation tool — on **Ubuntu 24.04** using **Docker** and **Portainer**.  
This **step-by-step tutorial** will help you configure **n8n** with **HTTPS (SSL)** enabled for secure access, ensuring full control over your Docker containers using Portainer.

---

## 🚀 Prerequisites

### 💾 Software Requirements
- ✅ **Ubuntu 24.04** (fresh installation recommended)  
- ✅ **Docker** (v28+)  
- ✅ **Docker Compose Plugin**  
- ✅ **Portainer (CE)** for Docker management  
- ✅ **acme.sh** for free SSL certificates from Let's Encrypt  
- ✅ **Domain name** (e.g., `n8n.yourdomain.com`) pointing to your server's IP  

### 🖥️ Hardware Requirements (Recommended)
- 💾 2GB RAM (minimum)  
- 💿 10GB Disk space  
- 🌐 A public-facing server (for HTTPS verification)  

---

## 🏗️ Step-by-Step Installation

---

### 1️⃣ Update and Upgrade Your Server
```
sudo apt update -y && sudo apt upgrade -y
```

---

### 2️⃣ Install Docker and Docker Compose Plugin
```
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

✅ **Verify Docker installation:**
```
docker --version
```

---

### 3️⃣ Install Portainer (Docker Management UI)
```
docker volume create portainer_data

docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce:latest
```

🔗 Access Portainer at:  
```
https://[YOUR_SERVER_IP]:9443
```
🔑 Set admin credentials and connect to **local** Docker environment.

---

### 4️⃣ Install acme.sh (SSL Certificate Tool)
```
sudo apt install curl socat -y
curl https://get.acme.sh | sh
source ~/.bashrc

~/.acme.sh/acme.sh --set-default-ca --server letsencrypt
~/.acme.sh/acme.sh --register-account -m your-email@example.com
```

---

### 5️⃣ Generate SSL Certificates for n8n Domain
```
~/.acme.sh/acme.sh --issue -d n8n.yourdomain.com --standalone

sudo mkdir -p /etc/ssl/private /etc/ssl/certs

~/.acme.sh/acme.sh --installcert -d n8n.yourdomain.com \
--key-file /etc/ssl/private/n8n_private.key \
--fullchain-file /etc/ssl/certs/n8n_certificate.crt

sudo chown 1000:1000 /etc/ssl/private/n8n_private.key
sudo chown 1000:1000 /etc/ssl/certs/n8n_certificate.crt

sudo chmod 600 /etc/ssl/private/n8n_private.key
sudo chmod 644 /etc/ssl/certs/n8n_certificate.crt
```

---

### 6️⃣ Deploy n8n Using Portainer (Full Access & SSL Enabled)

#### 🛠 Steps in Portainer:
1. Go to **Volumes** → **+ Add volume** → Name it:  
```
n8n_data
```

2. Go to **Containers** → **+ Add container** → Fill in:  
```
Name: n8n
Image: docker.n8n.io/n8nio/n8n:latest
```

#### 🔌 Port Mapping:
```
Host: 8443
Container: 8443
```

#### 📂 Volumes:
```
container:/etc/ssl/private/n8n_private.key => Bind Mode
volume:/home/node/.n8n/private.key => Writable

container:/etc/ssl/certs/n8n_certificate.crt => Bind Mode
volume:/home/node/.n8n/certificate.crt => Writable

container:n8n_data => Volume Mode
volume:/home/node/.n8n => Writable
```

#### ⚡ Environment Variables (Env):
```
Name:N8N_PROTOCOL
value:https

Name:N8N_HOST
value:n8n.yourdomain.com

Name:N8N_PORT
value:8443

Name:N8N_SSL_KEY
value:/home/node/.n8n/private.key

Name:N8N_SSL_CERT
value:/home/node/.n8n/certificate.crt

Name:N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS
value:true

Name:N8N_RUNNERS_ENABLED
value:true
```

✅ **Click "Deploy the container"**

---

### 7️⃣ Verify and Access n8n
Access **n8n** via:  
```
https://n8n.yourdomain.com:8443
```

---

### 🌟 Maintenance & Updates

#### 🔄 Auto-renew SSL Certificate:
acme.sh handles auto-renewal. For manual renewal:  
```
~/.acme.sh/acme.sh --renew -d n8n.yourdomain.com --force
docker restart n8n
```

---

#### ⚡ Check Logs (For Debugging):
```
docker logs n8n
```

---

#### ♻️ Restart or Stop n8n:
```
docker restart n8n
docker stop n8n
```

---

### 🏃 🎉 Congratulations!
You have successfully installed and secured **n8n** with **Docker**, **Portainer**, and **SSL** on **Ubuntu 24.04**.  
Now, you’re ready to build powerful workflows with secure web access! 🚀✨

---

## 📬 Get in Touch

💬 **Have questions? Let’s talk!**  
🌐 **Website:** [https://ava-ertebat.ir](https://ava-ertebat.ir)  
📧 **Support Email:** support@ava-ertebat.ir  
📧 **Personal Email:** omidswordfish@gmail.com  
📱 **WhatsApp:** [Chat with me on WhatsApp](https://wa.me/989163422797?text=Hi%20there!%20I%20would%20like%20some%20help%20regarding%20n8n%20installation%20on%20Ubuntu%2024.04.)

---

✨ **Enjoy automating with n8n!** 🌟

