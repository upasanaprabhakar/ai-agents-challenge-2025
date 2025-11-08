# Setup Guide

Getting everything ready for the challenge.

## What You Need

1. **Python** (3.9 or newer)
   - Check: `python --version`
   - Download from: https://python.org

2. **Git**
   - Check: `git --version`
   - Download from: https://git-scm.com

3. **A code editor**
   - VS Code (recommended): https://code.visualstudio.com
   - Or use Jupyter directly

4. **Kaggle account**
   - Sign up at: https://www.kaggle.com

5. **Google Gemini API key**
   - Get yours at: https://makersuite.google.com/app/apikey



## Step-by-Step Setup

### 1. Clone This Repo

```bash
git clone https://github.com/yourusername/ai-agents-challenge-2025.git
cd ai-agents-challenge-2025
```

### 2. Create a Virtual Environment (Recommended)

**On Mac/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**On Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r config/dependencies.txt
```

### 4. Set Up Your API Key

Create a `.env` file in the root folder:

```bash
echo "GOOGLE_API_KEY=your_actual_api_key_here" > .env
```

**Important:** Never commit this file! It's already in `.gitignore`.

### 5. Test Your Setup

Try running this in Python:

```python
from dotenv import load_dotenv
import os
import google.generativeai as genai

load_dotenv()
genai.configure(api_key=os.getenv('GOOGLE_API_KEY'))

model = genai.GenerativeModel('gemini-pro')
response = model.generate_content("Say hello!")
print(response.text)
```

If you see a response, you're all set! 🎉



## Kaggle Setup

### For Running Notebooks on Kaggle

1. Go to your Kaggle account settings
2. Navigate to "Add-ons" > "Secrets"
3. Add a new secret:
   - Key: `GOOGLE_API_KEY`
   - Value: Your Gemini API key

In your notebooks, access it like this:

```python
from kaggle_secrets import UserSecretsClient
user_secrets = UserSecretsClient()
api_key = user_secrets.get_secret("GOOGLE_API_KEY")
```



## Troubleshooting

### "Command not found" errors
- Make sure Python is installed and in your PATH
- Try `python3` instead of `python` on Mac/Linux

### Module import errors
- Make sure your virtual environment is activated
- Try reinstalling: `pip install -r config/dependencies.txt --force-reinstall`

### API key issues
- Double-check your key is correct (no extra spaces)
- Make sure the `.env` file is in the right place
- Try printing `os.getenv('GOOGLE_API_KEY')` to debug

### Jupyter not launching
- Install it explicitly: `pip install jupyter notebook`
- Try: `jupyter notebook` or `python -m notebook`



## Quick Commands Reference

```bash
# Activate virtual environment
source venv/bin/activate  # Mac/Linux
venv\Scripts\activate     # Windows

# Install/update packages
pip install -r config/dependencies.txt

# Start Jupyter
jupyter notebook

# Run a Python file
python your_script.py

# Check what's installed
pip list

# Deactivate virtual environment when done
deactivate
```



## Tips

- Keep your virtual environment activated while working
- Update packages if something isn't working: `pip install --upgrade package-name`
- If you're on Kaggle, you don't need the local setup - just use their notebooks directly
- Save your work frequently and commit to Git!



Need help? Check the Kaggle Discord or open an issue on this repo.