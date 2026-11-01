# EcoScan: AI-Powered Local Recycling Assistant

Final project for the Building AI course

## Summary
EcoScan is a mobile app using computer vision to identify household waste and provide location-specific recycling instructions. This Building AI course project aims to reduce recycling contamination and promote sustainable community habits.

## Background
Recycling confusion is a major global environmental issue. Studies show that up to 25% of items in residential recycling bins are contaminated, which can render entire batches of recyclables unfit for processing and send them to landfills. 
* **The Problem:** Ambiguous packaging and varying local municipal rules confuse consumers.
* **Personal Motivation:** I am personally motivated by seeing perfectly good recyclable materials discarded incorrectly in my own community due to simple labeling confusion.
* **Importance:** Improving recycling accuracy directly reduces landfill waste, conserves resources, and lowers carbon footprints, making it a highly impactful, scalable solution.

## How is it used?
The solution is designed as a smartphone application used at the point of disposal (e.g., in the kitchen or at a public trash bin). 
1. The user opens the app and points their camera at an item (e.g., a specific plastic container or coffee cup).
2. The AI identifies the material and item type.
3. The app uses the device's location to cross-reference local municipal recycling guidelines.
4. It provides a clear, actionable instruction: "Recycle", "Compost", or "Landfill", along with preparation steps (e.g., "Rinse and remove plastic lid").

*Target Users:* Environmentally conscious citizens, families, and local municipalities looking to improve waste management metrics.

![Recycling Concept](https://upload.wikimedia.org/wikipedia/commons/thumb/9/93/Recycling_symbol.svg/1200px-Recycling_symbol.svg.png)

## Data sources and AI methods
* **Image Data:** The model will be trained on open-source waste image datasets, such as the [TACO (Trash Annotations in Context) Dataset](https://github.com/pedropro/TACO), which contains thousands of annotated images of litter and waste.
* **Guideline Data:** Municipal recycling rules will be sourced from open city data portals (CSV/JSON formats) or web-scraped from official city websites.
* **AI Techniques:** 
  * **Computer Vision:** A lightweight Convolutional Neural Network (CNN) like MobileNetV2 will be used for on-device image classification to ensure fast, offline-capable predictions.
  * **Natural Language Processing (NLP):** Basic keyword matching or a small transformer model to parse unstructured municipal text guidelines into structured, machine-readable rules.


  * Challenges
    
Technical Limitations: Real-world conditions like poor lighting, crumpled items, or occlusion can reduce image classification accuracy.
Data Maintenance:

 Municipal recycling rules change frequently. Keeping the database updated requires automated scraping pipelines or official API partnerships.
Ethical Considerations:

The app requires location data to provide accurate local rules. This raises privacy concerns. The solution must process location data locally on the device or strictly anonymize it, ensuring no personal tracking or data selling occurs.

What next?

To evolve from a prototype to a fully deployed community tool, the project needs:
Partnerships: Collaboration with local city councils to access official, real-time waste management APIs.
Feature Expansion: Integrating barcode scanning as a fallback when the camera cannot visually identify the object.
Gamification: Adding community leaderboards and rewards to encourage consistent use.
To achieve this, I would seek assistance from backend developers for database management and UX/UI designers to make the app accessible to all age groups.

Acknowledgments

TACO Dataset: Trash Annotations in Context by Pedro Proença / MIT License.
Inspiration: Concepts from existing sustainability apps like Recycle Coach, but reimagined with a primary focus on on-device AI computer vision for a frictionless user experience.
Course Material: The "Building AI" course by the University of Helsinki and Reaktor Innovations for providing the foundational knowledge to structure this AI solution responsibly.

 Here is a simplified example of how the classification and rule-matching logic might be structured in Python:
```python
import tensorflow as tf
import json

# Load pre-trained lightweight model for on-device inference
model = tf.keras.models.load_model('ecoscan_mobilenet.h5')
def predict_item(image_tensor):
    # Get prediction probabilities from the neural network
    predictions = model.predict(image_tensor)
  class_idx = tf.argmax(predictions[0]).numpy()   
  # Map index to item category
    categories = ['plastic_bottle', 'cardboard', 'glass_jar', 'landfill_trash']
    item = categories[class_idx]
    
    return item

def get_local_rule(item, city_zip_code):
    # Fetch local rules from a structured JSON database
    with open('municipal_rules.json') as f:
        rules = json.load(f)
    
    city_rules = rules.get(city_zip_code, {})
    return city_rules.get(item, "Check local guidelines manually.")

# Example usage:
# identified_item = predict_item(user_camera_input)
# action = get_local_rule(identified_item, "90210")
# print(f"Action: {action}")

