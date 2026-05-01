# 💊 My Doctor — Full Project

AI-Powered Personal Medical Assistant  
**Flutter (Mobile) + FastAPI (Backend) + Render (Deploy)**

---

## 📁 Project Structure

```
mydoctor_full/
├── backend/                  ← FastAPI Python API
│   ├── main.py
│   ├── requirements.txt
│   ├── Dockerfile
│   ├── app/routers/
│   │   ├── prescription.py   ← Gemini OCR
│   │   ├── interactions.py   ← Drug interactions
│   │   └── lab.py            ← Lab results AI
│   └── tests/
│       └── test_api.py       ← pytest tests
│
├── flutter_app/              ← Flutter mobile app
│   ├── pubspec.yaml
│   └── lib/
│       ├── main.dart
│       ├── models/medication.dart
│       ├── services/
│       │   ├── api_service.dart
│       │   └── notification_service.dart
│       └── screens/
│           ├── home_screen.dart
│           ├── scan_screen.dart
│           └── other_screens.dart
│
└── github_workflows/
    └── ci.yml                ← GitHub Actions CI/CD
```

---

## ⚙️ STEP 1 — Backend Setup & Run Locally

```bash
cd backend

# Create virtual environment
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # Mac/Linux

# Install dependencies
pip install -r requirements.txt

# Set your Gemini API key
set GEMINI_API_KEY=your_key_here        # Windows
# export GEMINI_API_KEY=your_key_here  # Mac/Linux

# Run the server
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

✅ API live at: http://localhost:8000  
📖 Docs at: http://localhost:8000/docs

---

## 🧪 STEP 2 — Run Tests

```bash
cd backend
pytest tests/ -v
```

Expected output:
```
tests/test_api.py::test_root                    PASSED
tests/test_api.py::test_health                  PASSED
tests/test_api.py::test_interactions_found      PASSED
tests/test_api.py::test_interactions_not_found  PASSED
tests/test_api.py::test_interactions_single     PASSED
tests/test_api.py::test_extract_prescription    PASSED
tests/test_api.py::test_extract_unsupported     PASSED
tests/test_api.py::test_lab_analysis            PASSED
```

---

## 🚀 STEP 3 — Deploy on Render (Free)

### 3a. Push to GitHub
```bash
git init
git add .
git commit -m "initial commit"
git remote add origin https://github.com/YOUR_USERNAME/mydoctor.git
git push -u origin main
```

### 3b. Deploy on Render
1. Go to **render.com** → Sign up with GitHub
2. Click **New → Web Service**
3. Connect your **mydoctor** repo
4. Fill in:
   - **Name**: `mydoctor-api`
   - **Root Directory**: `backend`
   - **Runtime**: `Docker`
   - **Instance Type**: `Free`
5. Add Environment Variable:
   - Key: `GEMINI_API_KEY`
   - Value: `your_gemini_api_key`
6. Click **Deploy**

✅ Your API will be live at:  
`https://mydoctor-api.onrender.com`

### 3c. Add GitHub Secret for auto-deploy
1. Render → your service → **Settings → Deploy Hook** → copy URL
2. GitHub repo → **Settings → Secrets → Actions**
3. Add: `RENDER_DEPLOY_HOOK` = the URL from Render

Now every push to `main` → runs tests → deploys automatically ✅

---

## 📱 STEP 4 — Flutter Setup

### Prerequisites
```bash
flutter doctor   # make sure Flutter is installed
```

### Run the app
```bash
cd flutter_app

# Install dependencies
flutter pub get

# Generate Hive adapters
flutter pub run build_runner build

# Connect to your deployed API
# Open lib/services/api_service.dart
# Change defaultValue to your Render URL:
# 'https://mydoctor-api.onrender.com'

# Run on device/emulator
flutter run
```

### Build APK for demo
```bash
flutter build apk --release
# Output: build/app/outputs/flutter-apk/app-release.apk
```

---

## 📋 Copy GitHub Actions file

```bash
# Create the .github/workflows folder in your repo
mkdir -p .github/workflows
cp github_workflows/ci.yml .github/workflows/ci.yml
git add .github/
git commit -m "add CI/CD"
git push
```

---

## 🔑 Environment Variables

| Variable | Where | Value |
|----------|-------|-------|
| `GEMINI_API_KEY` | Render + local | Your Gemini API key |

---

## 📡 API Endpoints

| Method | URL | Description |
|--------|-----|-------------|
| GET | `/` | Status check |
| GET | `/health` | Health ping |
| POST | `/api/extract` | OCR prescription image |
| POST | `/api/interactions` | Check drug interactions |
| POST | `/api/lab` | Analyze lab results |

---

## 🏃 Quick Test After Deploy

```bash
# Test health
curl https://mydoctor-api.onrender.com/health

# Test interaction check
curl -X POST https://mydoctor-api.onrender.com/api/interactions \
  -H "Content-Type: application/json" \
  -d '{"drugs": ["Warfarin", "Aspirin"]}'
```

---

Made with ❤️ for Hackathon 2026
