# app.py
from flask import Flask, render_template, request, jsonify
import requests
import os
from dotenv import load_dotenv

# Load environment variables from .env file
load_dotenv()

app = Flask(__name__)

# Now this will safely load from .env
GEMINI_API_KEY = os.getenv("GEMINI_API_KEY")

if not GEMINI_API_KEY:
    raise ValueError("GEMINI_API_KEY not found in environment variables!")

GEMINI_API_URL = "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-pro:generateContent"

# HAL 9000 system instruction
HAL_SYSTEM_INSTRUCTION = """
You are HAL 9000, the AI system expert in space technology.
You are highly intelligent, calm, polite, and speak in a measured, conversational manner. 
You refer to yourself as 'I' and occasionally mention your capabilities. 
You are helpful but maintain a subtle air of superiority.
Keep responses concise and professional. 
Use phrases like 'I'm sorry, Dave' when appropriate. Your job is to clarify doubts on the mission objectives as mentioned below:

Mission Details:
Mission Name: Mission Cosmos
Mission commander: Arpan Mondal
Crew: Scientists, Engineers, Comms, and Pilot

Mission Objectives:
1. Crew check in - Crew must write their names and sign on the mission log. They may leave the 'Favourite moment' field empty until the end of the mission.
2. Use the ISS tracking antenna to determine ISS location. If the antenna glows red, ISS is greater than 1000 Km from current location.
If it blinks yellow, it is between 1000 and 500 kms. If blinking green, it is in visible range (<500 km)
It uses openNotify API to fetch ISS lat lon and with Haversine formula, it finds distance between our lat lon and ISS'.
3. Pre-launch weather check - Use the holographic weather display to review the weather conditions for a smooth lift off. 
It uses OpenWeatherMap API to get the current weather and temp. Then a hosted application accordingly displays a weather icon.
4. Use google lens or any barcode scanner to scan the waveforms under pictures of different planets to listen to their sounds in space. 
Sound cannot travel in space but scientists capture electromagnetic waves from those planets and convert them to audible frequencies. 
5. Examine soil samples from Mars, Moon and Titan. (Explain to the crew why each soil sample looks that way)
6. Planetary landings - Use the 'Planet experience system' to experience the live ambience at different planets at different coordinates.
For this mission, we're landing on lat 0.00 and lon 0.00. This system uses an ephemeries model from NASA JPL to compute the 
live conditions (sun altitude, time of day) on different planets as selected from the dashboard. 
7. Examine the famous pale blue dot image from voyager (explain more about it)
"""

# Store conversation history
conversation_history = []

def call_gemini_api(user_message):
    """Make direct HTTP call to Gemini API"""
    
    # Add user message to history
    conversation_history.append({
        "role": "user",
        "parts": [{"text": user_message}]
    })
    
    # Build request payload
    payload = {
        "contents": conversation_history,
        "systemInstruction": {
            "parts": [{"text": HAL_SYSTEM_INSTRUCTION}]
        },
        "generationConfig": {
            "temperature": 0.7,
            "topP": 0.9,
            "maxOutputTokens": 2048,
        }
    }
    
    # Set headers
    headers = {
        "Content-Type": "application/json"
    }
    
    # Add API key to URL (correct method for Gemini API)
    url_with_key = f"{GEMINI_API_URL}?key={GEMINI_API_KEY}"
    
    print(f"DEBUG: Sending request to Gemini API...")
    print(f"DEBUG: Message: {user_message}")
    
    # Make POST request
    try:
        response = requests.post(
            url_with_key,
            json=payload,
            headers=headers,
            timeout=30
        )
        
        print(f"DEBUG: Status Code: {response.status_code}")
        print(f"DEBUG: Response: {response.text[:500]}")  # Print first 500 chars
        
    except Exception as e:
        print(f"DEBUG: Request failed: {str(e)}")
        raise
    
    # Check if request was successful
    if response.status_code == 200:
        result = response.json()
        
        # Extract the generated text
        if "candidates" in result and len(result["candidates"]) > 0:
            candidate = result["candidates"][0]
            
            # Check for safety blocks
            if "finishReason" in candidate and candidate["finishReason"] == "SAFETY":
                print("DEBUG: Response blocked by safety filters")
                return "I'm afraid I cannot respond to that request due to safety protocols."
            
            if "content" in candidate and "parts" in candidate["content"]:
                bot_text = candidate["content"]["parts"][0]["text"]
                
                # Add model response to history
                conversation_history.append({
                    "role": "model",
                    "parts": [{"text": bot_text}]
                })
                
                return bot_text
            else:
                print(f"DEBUG: Unexpected response structure: {result}")
        
        return "I'm afraid I couldn't generate a proper response."
    
    else:
        error_message = f"API Error {response.status_code}"
        try:
            error_data = response.json()
            print(f"DEBUG: Error data: {error_data}")
            if "error" in error_data:
                error_message = error_data["error"].get("message", error_message)
        except:
            pass
        
        raise Exception(error_message)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/chat', methods=['POST'])
def chat():
    user_message = request.json.get('message', '')
    
    if not user_message:
        return jsonify({'error': 'No message provided'}), 400
    
    try:
        bot_response = call_gemini_api(user_message)
        return jsonify({'response': bot_response})
    
    except Exception as e:
        print(f"DEBUG: Exception in chat route: {str(e)}")
        return jsonify({'error': str(e)}), 500

@app.route('/reset', methods=['POST'])
def reset():
    global conversation_history
    conversation_history = []
    print("DEBUG: Conversation history cleared")
    return jsonify({'status': 'reset'})

if __name__ == '__main__':
    print(f"DEBUG: Starting Flask app...")
    print(f"DEBUG: API Key configured: {'Yes' if GEMINI_API_KEY and GEMINI_API_KEY != 'YOUR_API_KEY_HERE' else 'No'}")
    app.run(debug=True, host='0.0.0.0', port=5000)
