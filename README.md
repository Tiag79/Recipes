# Recipes: This is a text generative App from the Generative AI for Begginners on gitHub course that I have modified
import os
from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import SystemMessage, UserMessage
from azure.core.credentials import AzureKeyCredential

token = os.environ["GITHUB_TOKEN"]
endpoint = "https://models.inference.ai.azure.com"

model_name = "gpt-4o"

client = ChatCompletionsClient(
    endpoint=endpoint,
    credential=AzureKeyCredential(token),
)

no_recipes = input("No of recipes (for example, 5): ")

ingredients = input("List of ingredients (for example, chicken, potatoes, and carrots): ")

# interpolate the number of recipes into the prompt an ingredients
prompt = f"Show me {no_recipes} recipes for a dish with the following ingredients: {ingredients}.For each recipe, please provide:  1. Recipe name2. List of all ingredients with quantities3. Preparation time and cooking time.4.step-by-step cooking instructions5.Serving size6.Optional:cooking tips or variations"


response = client.complete(
    messages=[
        {
            "role": "system",
            "content": "You are a professional chef assistant. Provide detailed, clear recipes. with precise measurements and thorough cooking instructions. Include important cooking techniques, temperatures, and timing. Ensure safety in food handling and cooking temperatures.",
            
        
        },
        {
            "role": "user",
            "content": prompt,
        },
    ],
    model=model_name,
    # Optional parameters
    temperature=1.,
    max_tokens=1000,
    top_p=1.    
)

print(response.choices[0].message.content)
