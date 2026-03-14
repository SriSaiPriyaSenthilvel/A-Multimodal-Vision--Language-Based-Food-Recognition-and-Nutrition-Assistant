# A-Multimodal-Vision--Language-Based-Food-Recognition-and-Nutrition-Assistant
Key Features

    Zero-Shot Food Recognition — CLIP identifies food from any photo without task-specific training
    USDA Nutrition Lookup — Full nutritional breakdown (14 nutrients) from 7,700+ foods
    Semantic Recipe Search — FAISS-powered RAG retrieves relevant recipes from 2.2M recipe dataset
    Conversational Q&A — Flan-T5 LLM answers health, diet, and recipe questions grounded in real data
    Permanent Public Demo — Deployed on HuggingFace Spaces, accessible anytime

Architecture

┌─────────────────────────────────────────────────────────┐
│                   User Interface (Gradio)                │
└──────────────────────────┬──────────────────────────────┘
                           │
              ┌────────────▼────────────┐
              │   Food Photo Upload     │
              └────────────┬────────────┘
                           │
         ┌─────────────────▼──────────────────┐
         │     CLIP (openai/clip-vit-base)     │
         │     Zero-shot Food Recognition      │
         └─────────────────┬──────────────────┘
                           │ food_name + confidence
          ┌────────────────┼────────────────┐
          │                │                │
┌─────────▼──────┐ ┌───────▼───────┐ ┌─────▼────────────┐
│  USDA Nutrition │ │  FAISS Index  │ │   Flan-T5 LLM    │
│  Database Lookup│ │  Recipe RAG   │ │  Q&A Chatbot     │
│  (7,700+ foods) │ │ (2.2M recipes)│ │  (grounded)      │
└─────────┬───────┘ └───────┬───────┘ └─────┬────────────┘
          └────────────────►│◄──────────────┘
                    ┌───────▼───────┐
                    │  Final Output │
                    │  Nutrition +  │
                    │  Recipes + AI │
                    └───────────────┘

Datasets
Dataset 	Source 	Size 	Purpose
food.csv 	USDA FoodData Central (SR Legacy) 	7,793 foods 	Food name & category lookup
nutrient.csv 	USDA FoodData Central 	474 nutrients 	Nutrient definitions & units
food_nutrient.csv 	USDA FoodData Central 	~500K rows 	Nutrient values per food
food_category.csv 	USDA FoodData Central 	25 categories 	Category labels
full_dataset.csv 	Recipe Dataset 	2.2M recipes 	Semantic recipe matching

    Download USDA data free from fdc.nal.usda.gov → SR Legacy CSV

Models
Model 	Role 	Parameters
openai/clip-vit-base-patch32 	Food image recognition 	151M
google/flan-t5-large 	Nutrition Q&A chatbot 	780M
all-MiniLM-L6-v2 	Recipe semantic search 	22M
Notebooks

Run these in order in Google Colab:
Step 	Notebook 	Description
1 	Step1_Data_Pipeline.ipynb 	Merge USDA datasets → master nutrition table
2 	Step2_Food_Image_Recognition.ipynb 	CLIP food recognition pipeline
3 	Step3_Recipe_Matching_RAG.ipynb 	FAISS recipe search engine
4 	Step4_Chatbot_and_App.ipynb 	LLM chatbot + Gradio demo
Setup
Option A — Use the Live Demo (Recommended)

🔬 How It Works
Step 1 — Data Pipeline

All USDA datasets are merged into a single wide-format master table with one row per food and one column per nutrient. The recipe dataset is cleaned and ingredients are parsed into structured lists.
Step 2 — Food Recognition (CLIP)

CLIP encodes the uploaded image and 90+ food label prompts into a shared embedding space. Cosine similarity with temperature scaling identifies the best matching food label.
Step 3 — Recipe Matching (FAISS RAG)

All 50,000 recipe texts are encoded using all-MiniLM-L6-v2 and stored in a FAISS flat index. At query time, the food name is encoded and the top-K most semantically similar recipes are retrieved in milliseconds.
Step 4 — Nutrition Q&A (Flan-T5)

The identified food name, USDA nutrition data, and matched recipes are assembled into a structured prompt. Flan-T5 generates grounded, factual answers to any nutrition or recipe question.
Results
Metric 	Value
Food categories supported 	90+
USDA foods in database 	7,793
Recipes indexed 	50,000
Nutrients tracked per food 	14
Average inference time (CPU) 	~3s
Future Work

    Fine-tune CLIP on a dedicated food image dataset (Food-101, Food-500)
    Add meal tracking across a full day
    Personalised health scoring based on user profile
    Ingredient-level breakdown from dish photos
    Multilingual support for regional foods
    
https://private-user-images.githubusercontent.com/217276223/556518477-47e9d275-3e02-4898-b478-21ff79ea8832.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NzM0ODc3MzcsIm5iZiI6MTc3MzQ4NzQzNywicGF0aCI6Ii8yMTcyNzYyMjMvNTU2NTE4NDc3LTQ3ZTlkMjc1LTNlMDItNDg5OC1iNDc4LTIxZmY3OWVhODgzMi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYwMzE0JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MDMxNFQxMTIzNTdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05MWJiZDRkNGY1MTNlMTQ4MjM2N2IzZjY5ZDczYjM1MmVmY2VlMDJlZWY0ZDdhNDU0ZmE4Yzc0NDhiN2E1YWI4JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.R2_B3hg6D6NUwSI2hnxYGz14NlKKJETrxdCNgKMj6Hg
