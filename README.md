# 🤖 Sakai Auto Quiz Bot

An intelligent automation bot that logs into the **University of Ghana Sakai LMS**, detects available quizzes across your courses, and automatically answers them using **OpenAI (GPT-3.5-turbo)**.

Handles radio button questions, "select all that apply" checkboxes, paginated quizzes (one question per page), and multi-question pages. Built with Selenium and Python.

---

## ✨ Features

- 🔐 Auto-login to Sakai with your credentials
- 📚 Scans multiple course quiz pages automatically
- 🧠 Uses OpenAI to answer every question intelligently
- ✅ Handles both single-choice (radio) and multi-choice (checkbox) questions
- 📄 Works with paginated quizzes (Next button) and all-on-one-page layouts
- 🤝 Checks the Honor Pledge checkbox automatically before starting
- 🧩 Human-like random delays between answers to avoid detection
- 🔄 Automatically recovers from Sakai's AJAX page refreshes (stale elements)
- 🕵️ Spoofs browser User-Agent to appear as a real Chrome browser

---

## 📋 Requirements

- A computer running **macOS, Windows, or Linux**
- **Python 3.9 or higher**
- **Google Chrome** browser installed
- An **OpenAI API key** (get one at [platform.openai.com](https://platform.openai.com))
- Your **Sakai student ID and password**

---

## 🐍 Step 1 — Install Python

### macOS
1. Go to [python.org/downloads](https://www.python.org/downloads/)
2. Click **"Download Python 3.x.x"** (latest version)
3. Open the downloaded `.pkg` file and follow the installer
4. Verify installation by opening Terminal and typing:
   ```bash
   python3 --version
   ```

### Windows
1. Go to [python.org/downloads](https://www.python.org/downloads/)
2. Click **"Download Python 3.x.x"**
3. Open the installer — **make sure to check "Add Python to PATH"** before clicking Install
4. Verify by opening Command Prompt and typing:
   ```bash
   python --version
   ```

---

## 📦 Step 2 — Clone the Repository

```bash
git clone https://github.com/jasonOBGh/Sakai-Auto-Quiz.git
cd Sakai-Auto-Quiz
```

---

## 🌱 Step 3 — Create a Virtual Environment

A virtual environment keeps the project's dependencies isolated from the rest of your system.

### macOS / Linux
```bash
python3 -m venv venv
source venv/bin/activate
```

### Windows
```bash
python -m venv venv
venv\Scripts\activate
```

You'll know it's active when you see `(venv)` at the start of your terminal line.

---

## 📥 Step 4 — Install Dependencies

With the virtual environment active, run:

```bash
pip install selenium openai webdriver-manager python-dotenv
```

This installs:
| Package | Purpose |
|---|---|
| `selenium` | Controls the Chrome browser automatically |
| `openai` | Connects to the OpenAI API to answer questions |
| `webdriver-manager` | Automatically downloads the correct ChromeDriver |
| `python-dotenv` | Loads your credentials securely from the `.env` file |

---

## 🔑 Step 5 — Get an OpenAI API Key

1. Go to [platform.openai.com](https://platform.openai.com)
2. Sign up for an account (you'll need to add a payment method, but the costs are minimal)
3. Click **"API Keys"** in the left sidebar
4. Click **"Create new secret key"**, give it a name, and copy the key

> OpenAI offers a generous free tier for new users, and usage costs are very low for quiz answering.

---

## ⚙️ Step 6 — Create Your `.env` File

The `.env` file stores your personal credentials. It is **never uploaded to GitHub** (it's listed in `.gitignore` for your security).

Create a file named `.env` in the project folder and fill it in like this:

```env
# Your Sakai Student ID (the number you use to log in)
USERNAME=your_student_id_here

# Your Sakai password
PASSWORD=your_password_here

# Your OpenAI API key (get it from platform.openai.com)
OPENAI_API_KEY=your_openai_api_key_here
```

**Example:**
```env
USERNAME=20012345
PASSWORD=mypassword123
OPENAI_API_KEY=sk-...your_key_here
```

> ⚠️ **Never share your `.env` file or commit it to GitHub.** It contains your personal login credentials.

---

## 🌐 Step 7 — Set Your Course Quiz Page URLs

Open `sakai_quiz_bot.py` in any text editor (e.g. VS Code) and find the `QUIZ_PAGES` section near the top of the file:

```python
QUIZ_PAGES = [
    {"title": "MATH 233", "url": "https://sakai.ug.edu.gh/portal/site/MATH-223-.../jsf/index/mainIndex"},
    {"title": "DCIT 203", "url": "https://sakai.ug.edu.gh/portal/site/DCIT-203-.../jsf/index/mainIndex"},
    {"title": "DCIT 211", "url": "https://sakai.ug.edu.gh/portal/site/DCIT-211-.../jsf/index/mainIndex"},
    {"title": "DCIT 201", "url": "https://sakai.ug.edu.gh/portal/site/DCIT-201-.../jsf/index/mainIndex"},
    {"title": "DCIT 207", "url": "https://sakai.ug.edu.gh/portal/site/DCIT-207-.../jsf/index/mainIndex"},
]
```

**Replace each URL with your own course's Tests & Quizzes page URL.** Here's how to find it:

1. Log in to Sakai at [sakai.ug.edu.gh](https://sakai.ug.edu.gh)
2. Go to one of your courses
3. Click **"Tests & Quizzes"** in the left sidebar
4. Copy the full URL from your browser's address bar
5. Paste it into the `QUIZ_PAGES` list, replacing the existing URL for that course

You can add as many courses as you want, or remove ones you don't need:

```python
QUIZ_PAGES = [
    {"title": "My Course Name", "url": "https://sakai.ug.edu.gh/portal/site/YOUR-COURSE-ID/tool/YOUR-TOOL-ID/jsf/index/mainIndex"},
    # Add more courses here...
]
```

---

## 🚀 Step 8 — Run the Bot

With your virtual environment active:

### macOS / Linux
```bash
source venv/bin/activate
python3 sakai_quiz_bot.py
```

### Windows
```bash
venv\Scripts\activate
python sakai_quiz_bot.py
```

A Chrome window will open, log in automatically, and start working through your quizzes. Watch the terminal for live progress updates.

---

## 📁 Project Structure

```
Sakai-Auto-Quiz/
│
├── sakai_quiz_bot.py     # Main bot script
├── .env                  # Your credentials (NOT uploaded to GitHub)
├── .gitignore            # Tells git to ignore .env, venv, etc.
├── README.md             # This file
└── venv/                 # Virtual environment (NOT uploaded to GitHub)
```

---

## 🛠️ Troubleshooting

| Problem | Fix |
|---|---|
| `ModuleNotFoundError: No module named 'openai'` | Run `pip install openai` with venv active |
| `source: no such file or directory: venv/bin/activate` | Run `python3 -m venv venv` first to create the venv |
| Bot logs in but finds no quizzes | Check that the URLs in `QUIZ_PAGES` are correct for your courses |
| Chrome opens but login fails | Your Sakai password may have changed — update your `.env` file |
| `[!] No Next or Submit button found` | The quiz layout may be different — open an issue on GitHub |

---

## ⚠️ Disclaimer

This tool is intended for personal use and educational exploration of browser automation. Use it responsibly and in accordance with your institution's academic integrity policies. The author is not responsible for any academic consequences resulting from misuse of this tool.

---

## 🙏 Credits

Built with:
- [Selenium](https://selenium.dev) — browser automation
- [OpenAI](https://openai.com) — AI API for intelligent question answering
- [GPT-3.5-turbo](https://platform.openai.com/docs/models/gpt-3-5-turbo) — the AI model answering your questions
- [webdriver-manager](https://github.com/SergeyPirogov/webdriver_manager) — automatic ChromeDriver management