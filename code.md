# wokzok-responsible-ai
# ==========================================================
# RESPONSIBLE AI INSIGHT ENGINE
# Using FREE APIs / Open Source Models
# ==========================================================

from flask import Flask, request, jsonify
from transformers import pipeline
import requests
import re

# ----------------------------------------------------------
# Flask Application
# ----------------------------------------------------------

app = Flask(__name__)

# ----------------------------------------------------------
# Hugging Face FREE Transformer Model
# No API Key Required
# ----------------------------------------------------------

toxicity_detector = pipeline(
    "text-classification",
    model="unitary/toxic-bert"
)

# ----------------------------------------------------------
# Harmful Word Dataset
# ----------------------------------------------------------

harmful_words = [
    "hate",
    "abuse",
    "violence",
    "racist",
    "toxic",
    "fraud",
    "threat"
]

# ----------------------------------------------------------
# Harmful Word Detection Function
# ----------------------------------------------------------

def detect_harmful_words(text):

    detected_words = []

    for word in harmful_words:

        pattern = r'\b' + re.escape(word) + r'\b'

        if re.search(pattern, text.lower()):
            detected_words.append(word)

    return detected_words

# ----------------------------------------------------------
# Risk Score Calculation
# ----------------------------------------------------------

def calculate_risk_score(score):

    if score >= 0.80:
        return "High Risk"

    elif score >= 0.50:
        return "Medium Risk"

    else:
        return "Low Risk"

# ----------------------------------------------------------
# FREE AI Suggestion API
# Using Dummy Public API
# ----------------------------------------------------------

def get_ai_suggestions(text):

    response = requests.get(
        "https://api.adviceslip.com/advice"
    )

    if response.status_code == 200:

        data = response.json()

        return data['slip']['advice']

    return "No suggestion available"

# ----------------------------------------------------------
# Main AI Analysis Function
# ----------------------------------------------------------

def analyze_text(user_text):

    # Toxicity Detection
    toxicity_result = toxicity_detector(user_text)

    toxicity_score = toxicity_result[0]['score']

    # Harmful Word Detection
    harmful_detected = detect_harmful_words(user_text)

    # Risk Level
    risk_level = calculate_risk_score(toxicity_score)

    # Free AI Suggestion
    ai_suggestion = get_ai_suggestions(user_text)

    return {

        "toxicity_score": round(toxicity_score, 2),

        "risk_level": risk_level,

        "harmful_words_detected": harmful_detected,

        "ai_suggestion": ai_suggestion
    }

# ----------------------------------------------------------
# API Endpoint
# ----------------------------------------------------------

@app.route('/analyze', methods=['POST'])

def analyze():

    data = request.json

    user_text = data.get("text")

    result = analyze_text(user_text)

    return jsonify({

        "status": "success",

        "result": result
    })

# ----------------------------------------------------------
# Home Route
# ----------------------------------------------------------

@app.route('/')

def home():

    return """
    <h1>Responsible AI Insight Engine</h1>
    <p>Free AI-powered Toxicity Detection System</p>
    """

# ----------------------------------------------------------
# Run Application
# ----------------------------------------------------------

if __name__ == "__main__":

    app.run(
        host="0.0.0.0",
        port=5000,
        debug=True
    )

# ==========================================================
# END OF PROJECT CODE
# ==========================================================AI-powered Responsible AI content analysis platform for toxicity, bias, plagiarism, and AI text detection.
# Wokzok AI Platform

An AI-powered Responsible AI web application developed as an MCA Final Year Project.

## Features

- Toxicity Detection
- Bias Detection
- AI-generated Text Detection
- Plagiarism Checking
- Report Generation

## Technologies Used

- React.js
- Node.js
- Express.js
- MongoDB
- AI APIs

## Objective

To create a responsible AI system capable of analyzing digital content for safety, authenticity, and ethical concerns.

## Project Type

MCA Final Year Major Project

## Author

Kuldeep Sah
