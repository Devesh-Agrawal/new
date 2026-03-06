import requests
import json

def emotion_detector(text_to_analyze):
    """
    Analyzes the provided text using Watson NLP Emotion Predict service.
    """
    # URL for the Watson Emotion Analysis service
    url = 'https://sn-watson-emotion.labs.skills.network'
    
    # Headers required by the service
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    
    # Input format for the API
    myobj = { "raw_document": { "text": text_to_analyze } }
    
    # Sending the POST request
    response = requests.post(url, json = myobj, headers=header)
    
    # Parsing the response
    formatted_response = json.loads(response.text)

    # Extracting individual emotion scores and finding the dominant emotion
    if response.status_code == 200:
        emotions = formatted_response['emotionPredictions'][0]['emotion']
        anger_score = emotions['anger']
        disgust_score = emotions['disgust']
        fear_score = emotions['fear']
        joy_score = emotions['joy']
        sadness_score = emotions['sadness']
        dominant_emotion = max(emotions, key=emotions.get)
    elif response.status_code == 400:
        # Handle cases with empty or invalid input
        anger_score = None
        disgust_score = None
        fear_score = None
        joy_score = None
        sadness_score = None
        dominant_emotion = None

    return {
        'anger': anger_score,
        'disgust': disgust_score,
        'fear': fear_score,
        'joy': joy_score,
        'sadness': sadness_score,
        'dominant_emotion': dominant_emotion
    }# new
