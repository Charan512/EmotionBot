# AI Mental Health Assistant - Backend API

This directory contains the FastAPI backend for the AI Mental Health Assistant application.

## Prerequisites
- Python 3.9+ 
- Local or globally installed `uvicorn`

## Setup Instructions

### 1. Install Dependencies
Install all required Python packages using pip:

```bash
pip install -r requirements.txt
```

*(Note: Depending on your environment, you may want to use a virtual environment before installing.)*

### 2. Environment Variables
Create a `.env` file in the root of the `backend` directory. The following environment variables are required for full functionality:

```env
# Twilio Required Credentials (For emergency calls)
TWILIO_ACCOUNT_SID=your_account_sid
TWILIO_AUTH_TOKEN=your_auth_token
TWILIO_PHONE_NUMBER=your_twilio_phone

# Allowed CORS Origins
FRONTEND_URL=https://your_frontend_url.com

# Bhashini Transcription Keys
BHASHINI_USER_ID=your_bhashini_id
BHASHINI_UDYAT_KEY=your_udyat_key
BHASHINI_INFERENCE_API_KEY=your_inference_api_key
```

### 3. Running the Server
You can start the FastAPI backend server using the following command:

```bash
uvicorn api:app --reload --port 8000
```

The API will then be accessible at `http://localhost:8000`. You can test functionality via the interactive Swagger docs at `http://localhost:8000/docs`.

<<<<<<< HEAD
### Notes
- **Models**: The application will automatically download lightweight text classification models on first startup. Please allow a minute or two on initial run.
- **Risk Model**: Ensure `vectorizer.pkl` and `risk_model.pkl` are present in this directory to load successfully.
=======
### 3. Exposing Local Backend for Vercel (Ngrok)
If you deploy the frontend to the internet (Vercel) while keeping the heavy AI backend running locally, you must run an Ngrok tunnel:
```bash
# In an active terminal window
npx ngrok http 8000
```
This gives you a public Forwarding URL (e.g., `https://untinned-houndy...ngrok-free.dev`). 
Set this as the `VITE_API_URL` environment variable inside your Vercel deployment settings. Then update the `.env` file in your `backend` so `FRONTEND_URL` allows your Vercel URL.

---

## 🛠️ Tech Stack & Architecture

### Backend
- **Framework**: FastAPI (Python)
- **Database**: SQLite (`chat_history.db`)
- **Libraries**: `torch`, `transformers`, `scikit-learn`, `joblib`, `bcrypt`, `twilio`, `pandas`

### AI / NLP Models
- **Sentiment**: `cardiffnlp/twitter-roberta-base-sentiment-latest`
- **Emotion**: `SamLowe/roberta-base-go_emotions`
- **Risk Classifier**: Custom trained TF-IDF Logistic Regression model (`risk_model.pkl` & `vectorizer.pkl`)
- **LLM**: `TinyLlama/TinyLlama-1.1B-Chat-v1.0` hosted locally for fast generation without API timeouts.

### Frontend
- **Framework**: React + Vite
- **Styling**: Vanilla CSS (`App.css`) with modern UI elements.
- **State Management**: React Context (`AuthContext.jsx`)
- **Icons**: `lucide-react`

---

## ✨ Key Features

- **Engaging UI/UX**: Includes a welcoming entry page and a fully featured Landing page explaining the mission, features, and FAQs.
- **Conversational AI**: Human-like interactions that adapt to user sentiment and emotional states.
- **Text-to-Speech (TTS)**: Integrated Web Speech API to read bot messages aloud in a calming voice.
- **Crisis De-escalation Protocol**: If high risk is detected, the AI uses a strict dynamic system prompt to actively de-escalate the user while *silently* triggering Twilio to place an emergency call to a designated contact. The call explicitly recites the chat message that triggered the alarm.
- **User Authentication**: Secure JWT-based login and registration system.
- **Chat History**: Saves and retrieves conversation history with distinct chat session tracking.

---

## 🔐 Model Retraining

If you want to append new data to the Suicide Risk Classifier:
1. Add new text samples to `backend/Suicide_Detection.csv`.
2. Run `python clean_dataset.py` (if you added any custom noise removal scripts) or let testing strip out URLs and references.
3. Run `python train_risk_model.py`.
4. Overwrite confirmation will appear, saving the new `risk_model.pkl`. 

## License
>>>>>>> 26ba271 (Migrate backend database from SQLite to MongoDB)
