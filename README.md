# Physician Notetaker

A powerful Natural Language Processing (NLP) system for automatically analyzing medical conversations and generating structured clinical documentation.

## Overview

Physician Notetaker is an end-to-end NLP pipeline designed to help healthcare providers save time and improve documentation quality by automatically processing medical conversation transcripts. The system can:

- Parse and structure physician-patient conversations
- Identify medical entities (symptoms, diagnoses, treatments, prognoses)
- Generate formatted medical reports and SOAP notes
- Analyze patient sentiment and intent throughout conversations
- Extract key information for clinical documentation

## Features

- **Conversation Parser**: Handles various conversation formats and speaker identification
- **Medical Entity Recognition**: Identifies medical terminology across multiple categories
- **Report Generator**: Creates structured medical reports from conversation data
- **Sentiment Analyzer**: Tracks patient sentiment and intent throughout the conversation
- **SOAP Note Generator**: Produces clinical documentation in the standard SOAP format
- **End-to-End Pipeline**: Combines all components for comprehensive analysis

## Installation

### Prerequisites

- Python 3.7+
- pip package manager

### Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/physician-notetaker.git
   cd physician-notetaker
   ```

2. Create and activate a virtual environment (recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Download necessary resources:
   ```bash
   python -m spacy download en_core_web_md
   python -m nltk.downloader punkt stopwords
   ```

## Usage

### Basic Usage

```python
from medical_nlp_pipeline import MedicalNLPPipeline

# Initialize the pipeline
pipeline = MedicalNLPPipeline()

# Process a medical conversation
with open('sample_conversation.txt', 'r') as f:
    conversation = f.read()

# Run the analysis
analysis = pipeline.process(conversation)

# Get JSON output
json_output = pipeline.generate_json_output(analysis)
print(json_output)
```

### Component-Level Usage

You can also use individual components:

```python
from medical_nlp_pipeline import ConversationParser, MedicalEntityRecognizer

# Parse conversation
parser = ConversationParser()
conversation_data = parser.parse(conversation)

# Extract medical entities
recognizer = MedicalEntityRecognizer()
entities = recognizer.identify_entities(conversation_data.get("all_patient_text", ""))
```

## Sample Output

### Parsed Conversation Structure

The conversation parser extracts structured data:

```json
{
  "physician_statements": [
    "Good morning. How are you feeling today?",
    "I'm sorry to hear that. Can you describe the pain?",
    "And any other symptoms along with the back pain?"
  ],
  "patient_statements": [
    "Not so great, doctor. My back has been killing me for the past two weeks after that car accident.",
    "It's a sharp pain in my lower back, especially when I try to bend over or get up from sitting. It's been getting slightly better over the last few days but still bothers me a lot.",
    "Sometimes I feel a tingling sensation down my left leg, and I've been having trouble sleeping because I can't find a comfortable position."
  ],
  "metadata": {
    "patient_name": "Patient"
  }
}
```

### Medical Entity Recognition

The system identifies key medical entities:

```json
{
  "SYMPTOM": [
    "back pain", 
    "sharp pain in my lower back", 
    "tingling sensation", 
    "trouble sleeping"
  ],
  "DIAGNOSIS": [
    "lumbar strain", 
    "nerve irritation"
  ],
  "TREATMENT": [
    "anti-inflammatory medication", 
    "muscle relaxants", 
    "gentle stretching exercises", 
    "physical therapist"
  ],
  "PROGNOSIS": [
    "significant improvement within 2-4 weeks"
  ]
}
```

### Generated SOAP Note

The SOAP note generator produces clinical documentation:

```json
{
  "Subjective": {
    "Chief_Complaint": "back pain, sharp pain in lower back",
    "History_of_Present_Illness": "Patient reports back pain for the past two weeks after car accident. Sharp pain in lower back, especially when bending or rising from sitting."
  },
  "Objective": {
    "Physical_Exam": "Physician examines the patient's back, checking for tenderness and mobility. Inflammation and tenderness in the lumbar region noted.",
    "Observations": "Based on patient report and examination"
  },
  "Assessment": {
    "Diagnosis": "lumbar strain, nerve irritation",
    "Severity": "Moderate, improving"
  },
  "Plan": {
    "Treatment": "anti-inflammatory medication, muscle relaxants, gentle stretching exercises, physical therapy",
    "Follow_Up": "schedule a follow-up appointment in two weeks"
  }
}
```

### Patient Sentiment Analysis

The sentiment analyzer tracks emotional progression:

```json
{
  "Overall_Sentiment": "Anxious, improving to Reassured",
  "Primary_Intent": "Seeking reassurance",
  "Sentiment_Progression": "Improving (Anxious → Reassured)",
  "Statement_Analysis": [
    {
      "Statement": "Is this serious? Will I need surgery?",
      "Analysis": {
        "Sentiment": "Anxious",
        "Intent": "Seeking reassurance",
        "Confidence": 0.87
      }
    },
    {
      "Statement": "Thank you doctor, I'm feeling more hopeful now.",
      "Analysis": {
        "Sentiment": "Reassured",
        "Intent": "Expressing gratitude",
        "Confidence": 0.92
      }
    }
  ]
}
```

## Methodology

### Conversation Parsing

The system employs a flexible parsing approach that:
- Identifies speaker roles using pattern matching
- Handles different conversation formats (labeled/unlabeled)
- Extracts metadata like patient names and physical examination notes
- Uses turn-taking patterns to handle unstructured conversations

### Medical Entity Recognition

Entity recognition combines multiple approaches:
1. **Rule-Based Detection**: Using medical terminology dictionaries
2. **spaCy NER**: Leveraging pre-trained medical entity recognition
3. **Context Analysis**: Examining surrounding text to improve accuracy
4. **Negation Detection**: Avoiding false positives from negated terms

The system uses expandable category dictionaries that can be customized for different medical specialties.

### Sentiment Analysis

The sentiment analyzer employs a hybrid approach:
- **Transformer-Based Model**: Using DistilBERT for base sentiment classification
- **Medical Context Enhancement**: Mapping general sentiment to medical-specific categories
- **Pattern Recognition**: Identifying intent patterns in patient statements
- **Temporal Analysis**: Tracking sentiment progression throughout the conversation

### SOAP Note Generation

The SOAP note generator uses structured extraction methods:
- Identifies chief complaints from symptoms and initial statements
- Extracts temporal information for history of present illness
- Analyzes physician statements for physical examination details
- Determines severity based on contextual indicators
- Formulates treatment plans from recommendation statements

## Future Enhancements

- Fine-tuning with specialty-specific medical vocabularies
- Integration with EHR systems
- Support for audio transcription input
- Multi-language support
- Interactive visualization dashboard

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- NLTK and spaCy for NLP foundations
- Hugging Face Transformers for sentiment analysis
- Medical NLP research community for entity recognition approaches
