---
title: " 📚 Semantic Book Recommendation"
excerpt: "An intelligent book recommendation tool that uses semantic similarity and natural language processing to help users discover books based on their interests or queries.<br/><img src='/images/project1.png' width='300'>"
collection: portfolio
---

As a book lover, I found this project really interesting and enjoyable to work on. This small project allowed me to learn how to apply natural language processing and semantic search techniques to build an intelligent book recommendation system. It also serves as my personal assistant when I need to find a new book to read. Of course, it would be better if the dataset were larger and included more books—but it's just a start and a foundation to build upon. [Demo](https://huggingface.co/spaces/sovanleng/semantic-book-recommender-demo)

The code for this project can be found [here](https://github.com/SovanlengLY/semantic-book-recommender).

> This project is based on the excellent work by [@t-redactyl](https://github.com/t-redactyl) and the tutorial ["Build an LLM-powered Book Recommendation System"](https://www.youtube.com/watch?v=Q7mS1VHm3Yw) available on YouTube.

# Technical Overview

This project utilizes the [7k Books Dataset](https://www.kaggle.com/datasets/dylanjcastillo/7k-books-with-metadata) from Kaggle, which contains approximately 7,000 books with metadata such as titles, authors, categories, descriptions, and ratings. This dataset provides a solid foundation for building and evaluating a book recommendation system using language models.

Below, I provide a technical breakdown of each core component implemented in the system.

---

## 1. Text Data Cleaning

High-quality data is fundamental for any NLP application. In this project, data cleaning consists of several systematic steps:

- **Handling Missing Values:**  
  All rows with missing critical fields (such as title, description, or author) are removed. Given that the percentage of missing data is low, dropping incomplete rows simplifies the pipeline and avoids potential bias from imputation.

- **Filtering by Description Length:**  
  Book descriptions under 25 words are excluded. Descriptions of insufficient length are unlikely to contain enough semantic information for reliable recommendations.

## 2. Semantic (Vector) Search

Semantic search refers to retrieving documents based on _meaning_, rather than exact keyword matching.

- **Text Embeddings:**
  Each book description and user query is encoded into a fixed-length vector using a sentence embedding model. In this implementation, [`paraphrase-MiniLM-L3-v2`](https://huggingface.co/sentence-transformers/paraphrase-MiniLM-L3-v2) is used for its efficiency and reasonable performance.

- **Vector Similarity:**
  Given a user query, the system encodes the query and computes cosine similarity with all precomputed book vectors. The top-N most similar books are selected as recommendations.

## 3. Text Classification (Zero-Shot Learning)

The system can classify books as "Fiction" or "Non-Fiction" without any task-specific labeled data.

- **Zero-Shot Classification:**
  This approach uses language models trained on natural language inference tasks to assign labels provided at runtime. The model [`"facebook/bart-large-mnli"`](https://huggingface.co/facebook/bart-large-mnli) is utilized through Hugging Face’s pipeline.

## 4. Sentiment and Emotion Analysis

Books can be filtered or ranked according to the sentiment or emotional content of their descriptions.

- **Emotion Extraction:**
  The model [`"j-hartmann/emotion-english-distilroberta-base"`](https://huggingface.co/j-hartmann/emotion-english-distilroberta-base) classifies text into seven emotions: anger, disgust, fear, joy, sadness, surprise, and neutral.

- **Procedure:**
  Each book description is split into sentences. Emotions are predicted for each sentence, and the dominant emotion for the book is assigned based on the highest cumulative score.

## 5. Creating a Web Application with Gradio

To make the system accessible, a web interface is implemented using Gradio.

- **Features:**

- Users can input free-text queries and select filters such as genre or emotion.

- The interface displays recommended books with their cover images and metadata.

- Emotion and genre filters are dynamically applied based on model outputs.

- **Deployment:**
  The [Gradio](https://gradio.app/) app can be run locally or hosted on platforms like [Hugging Face Spaces](https://huggingface.co/spaces) for broader access.
