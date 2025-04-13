# 🚀 Installing Tutor (OpenEdx/Tutor Fork for Fanos)

> 📅 Created by: **Shadi**  
> 🕒 Last updated: **13 April, 2025**  
> 🏷️ Tags: `Tutor`, `OpenEdx`, `Ubuntu`, `Docker`, `Education`

---

## 📌 Preface

> This guide walks through the process of installing Tutor with a **custom forked version of OpenEdx** for research purposes.  
> ✅ Tested on **Ubuntu 24.04 (Noble)** and **WSL/WSLG v2.0.0+**

To avoid installation issues, ensure your system is free from previous Tutor configurations:

```bash
find ~ -name '*tutor*'
```

If no results are returned, you're good to go.

---

## 🧰 Prerequisites

1. You must be on **Ubuntu 24.04 (Noble)** or WSL/WSLG v2.0.0+
2. Docker must be fully installed and running. Follow the official Docker documentation:

   👉 [Install Docker on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

> 💥 **Common Docker error:**

```bash
docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock.
```

To resolve:

```bash
sudo systemctl status docker
sudo docker run hello-world
# If issues persist:
sudo reboot
```

---

## 🧪 Virtual Environment & Research Fork Setup

It is **highly recommended** to use a virtual environment. Avoid system-wide package modifications like `break-system-packages`.

1. Create a directory and virtual environment:

```bash
mkdir OpenEdx
python3 -m venv OpenEdx/
source OpenEdx/bin/activate
cd OpenEdx
```

2. Clone the research fork of Tutor:

```bash
git clone https://github.com/shadielfares/OpenEdx .
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Launch Tutor:

```bash
tutor local launch
```

> ⚠️ At this stage, you may encounter Docker-related permission issues. If so:

```bash
sudo usermod -aG docker $USER
# Then restart the terminal completely
```

---

## 🐳 Common Docker Errors & Fixes

### ❌ Error: `docker-compose-f` or permission issues

Fix:

```bash
# Restart Docker services and re-run Tutor
sudo systemctl start docker
tutor local launch
```

---

### ❌ Error: External connectivity/port binding failure (e.g., port 80)

```bash
# Check for port conflicts
sudo lsof -i :80

# Stop any conflicting services
sudo systemctl stop nginx
sudo systemctl stop apache2

# Relaunch Tutor
docker compose -f ~/.local/share/tutor/env/local/docker-compose.yml \
               -f ~/.local/share/tutor/env/local/docker-compose.prod.yml \
               --project-name tutor_local up --remove-orphans -d
tutor local launch
```

---

## 📚 Understanding the OpenEdx Sites

1. **Search site** – Explore all domains and accessible courses.
2. **Studio** – Build, edit, and manage course content.
3. **LMS** – The student interface where users view enrolled courses.

---

## ✉️ Sending Emails (SMTP)

If you're not receiving confirmation/login emails, configure SMTP:

📘 Follow this guide: [Using Gmail as SMTP — Tutor Docs](https://docs.tutor.edly.io/tutorials/google-smtp.html)

Then edit your Tutor config:

```bash
cd ~/.local/share/tutor/
vim config.yml
```

Scroll to the end (`Shift + G`) and confirm SMTP settings match the guide.

Finally, create your admin account:

```bash
tutor local do createuser --superuser --staff admin admin@yourdomain.com
```

---

## 🏗️ Creating a Course

Go to Studio → Click “New Library” → Follow prompts to set up your course.

✅ **That's it! You're ready to start building with OpenEdx.**

---

## 🧯 Troubleshooting

If you run into any issues not covered here:

👉 **Please open a detailed GitHub issue** in the [repo](https://github.com/shadielfares/OpenEdx/issues) with your logs and system setup.
