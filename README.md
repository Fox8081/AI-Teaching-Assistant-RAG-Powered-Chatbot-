# AI Teaching Assistant for Video Courses using RAG
A powerful AI assistant that lets you "chat" with your video courses. Ask a question in natural language and get a direct answer with the exact video and timestamp.

[![Python](https://img.shields.io/badge/python-3.11-blue.svg)](https://www.python.org/)

## Table of Contents
- [Project Overview](#project-overview)
- [How It Works](#how-it-works)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Usage](#usage)
- [Contributing](#contributing)
---

## Project Overview

This project is a Retrieval-Augmented Generation (RAG) system designed to make video course content easily searchable and interactive. It processes a library of video lectures, transcribes the spoken content, and builds a searchable vector knowledge base. When a user asks a question, the system retrieves the most relevant segments from the videos and uses a Large Language Model (LLM) to generate a precise, helpful answer, complete with source timestamps. The entire pipeline runs locally using Ollama, ensuring privacy and eliminating the need for paid API keys.

## How It Works

The system is built on a five-step RAG pipeline that transforms raw video files into an interactive Q&A experience.

**1. Audio Extraction:** Videos from the /videos folder are processed by video_to_mp3.py, which uses ffmpeg to extract the audio track and save it as an MP3 file in the /audios directory.

**2. Transcription:** mp3_to_json.py uses the openai-whisper model to transcribe the audio files. It generates structured JSON files containing the full transcript broken down into text chunks with precise start and end timestamps.

**3. Vector Indexing:** preprocess_json.py reads the transcribed text chunks and converts each one into a numerical vector (embedding) using the bge-m3 model via Ollama. This collection of vectors is saved as a single embeddings.joblib file, which serves as our searchable knowledge base.

**4. Retrieval:** When a user asks a question, process_incoming.py converts the query into a vector and uses Cosine Similarity to compare it against all the vectors in the knowledge base, retrieving the top 5 most relevant text chunks.

**5. Generation:** The retrieved chunks and the original question are formatted into a detailed prompt. This is then fed to the llama3.2 LLM (via Ollama) to synthesize a final, human-readable answer that points the user to the exact source video and timestamp.

## Features

Automated Content Processing: A complete pipeline to automatically process video files into a searchable knowledge base.

Semantic Search: Leverages vector embeddings to understand the contextual meaning of questions, providing more accurate results than keyword search.

Precise Source Citing: All answers include the video number and timestamp, allowing users to jump directly to the relevant content.

Local & Private: Runs entirely on your local machine using Ollama, ensuring your data remains private and there are no API costs.

Extensible: Easily adaptable to different video courses or knowledge domains.

## Prerequisites
Python 3.9 or higher

ffmpeg

Ollama

## Installation
**1. Clone the repository**

git clone [https://github.com/your-username/ai-teaching-assistant.git](https://github.com/your-username/ai-teaching-assistant.git)
cd ai-teaching-assistant

**2. Create and activate a Python virtual environment**

python -m venv venv
### On macOS/Linux
source venv/bin/activate
### On Windows
.\\venv\\Scripts\\activate

**3. Install project dependencies**

pip install -r requirements.txt

**4. Setup Ollama and Models**

After installing Ollama, ensure the application is running. Then, pull the required models:

ollama pull llama3.2
ollama pull bge-m3

**5. Create Project Directories**

mkdir videos audios jsons

## Project Structure
videos/              :   Place your source video files here

audios/              :   Stores extracted MP3 files

jsons/               :   Stores transcribed JSON data

video_to_mp3.py      :   Script to convert video to audio

mp3_to_json.py       :   Script to transcribe audio to text

preprocess_json.py   :   Script to create vector embeddings

process_incoming.py  :   Main script to ask questions

embeddings.joblib    :   Generated knowledge base file

requirements.txt     :   Project dependencies

README.md

## Usage
**Step 1:** Add Videos

Place all your video files (.mp4, .mkv, etc.) into the /videos folder.

**Step 2:** Run the Processing Pipeline

Execute the scripts in order to build the knowledge base.

### 1. Convert videos to audio
python video_to_mp3.py

### 2. Transcribe audio to text (This can be time-consuming)
python mp3_to_json.py

### 3. Create vector embeddings
python preprocess_json.py

**Step 3:** Ask a Question

Run the main script to start the interactive session.

python process_incoming.py

You will be prompted to enter a question in the terminal. The final answer will be printed to the console and also saved in response.txt.

## Tech Stack
Language: Python

LLM & Embeddings: Ollama (llama3.2, bge-m3)

Transcription: openai-whisper

Core Libraries: pandas, numpy, scikit-learn, joblib, requests

Audio Processing: ffmpeg

## Contributing
Contributions are welcome! Please fork the repo, create a feature branch, and submit a pull request. For major changes, please open an issue first to discuss what you would like to change.
