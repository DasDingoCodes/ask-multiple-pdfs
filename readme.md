# MultiPDF Chat App

This project is a fork of Alejandro AO's **MultiPDF Chat App** ([Youtube tutorial](https://youtu.be/dXxQ0LR-3Hg)). 
The main difference is that it runs an LLM locally with GPU support instead of using OpenAIs services. 
There are also some other smaller changes.

## Introduction
------------
The MultiPDF Chat App is a Python application that allows you to chat with multiple PDF documents. You can ask questions about the PDFs using natural language, and the application will provide relevant responses based on the content of the documents. This app utilizes a language model to generate accurate answers to your queries. Please note that the app will only respond to questions related to the loaded PDFs.

## How It Works
------------

The application follows these steps to provide responses to your questions:

1. PDF Loading: The app reads multiple PDF documents and extracts their text content.

2. Text Chunking: The extracted text is divided into smaller chunks that can be processed effectively.

3. Language Model: The application utilizes a language model to generate vector representations (embeddings) of the text chunks (not the LLM used to generate the response).

4. Similarity Matching: When you ask a question, the app compares it with the text chunks and identifies the most semantically similar ones.

5. Response Generation: The selected chunks are passed to the language model, which generates a response based on the relevant content of the PDFs.

## Dependencies and Installation
----------------------------
Requirements:

 - CUBLAS/CUDA (https://developer.nvidia.com/cuda-downloads)
 - CMake (https://cmake.org/download/)
 - Environment with Python 3.10.12

Installation instructions for Windows:

1. Open Powershell (Anaconda Powershell if using Anaconda environment) 

2. Set up variables:
   ```
   $env:CMAKE_ARGS = "-DLLAMA_CUBLAS=on"
   $env:FORCE_CMAKE = 1
   ```
   Detailed instructions: https://github.com/abetlen/llama-cpp-python#installation-with-openblas--cublas--clblast--metal    
   Alternative: https://python.langchain.com/docs/integrations/llms/llamacpp#installation-with-windows

3. Install requirements:
   ```
   pip install -r requirements.txt
   ```


## Usage
-----
To use the MultiPDF Chat App, follow these steps:

1. Ensure that you have installed the required dependencies.

2. Run the `main.py` file using the Streamlit CLI. Execute the following command:
   ```
   streamlit run app.py
   ```

3. The application will launch in your default web browser, displaying the user interface.

4. Load multiple PDF documents into the app by following the provided instructions.

5. Ask questions in natural language about the loaded PDFs using the chat interface.
