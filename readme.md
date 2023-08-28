# MultiPDF Chat App

This project is a fork of Alejandro AO's **MultiPDF Chat App** ([Youtube tutorial](https://youtu.be/dXxQ0LR-3Hg)). 
The main difference is that it runs an LLM locally with GPU support via LlangChain's LlamaCpp class instead of using OpenAIs services. 
There are also some other smaller changes.

## Introduction
------------
The MultiPDF Chat App is a Python application that allows you to chat with multiple PDF documents. You can ask questions about the PDFs using natural language, and the application will provide relevant responses based on the content of the documents. This app utilizes a language model to generate accurate answers to your queries. Please note that the app will only respond to questions related to the loaded PDFs.

## How It Works
------------

The application follows these steps to provide responses to your questions:

1. PDF Loading: The app reads multiple PDF documents and extracts their text content.

2. Text Chunking: The extracted text is divided into smaller chunks that can be processed effectively.

3. Language Model: The application utilizes a language model to generate vector representations (embeddings) of the text chunks. This is a different language model than the one used to generate text, in this case [GTE-base](https://huggingface.co/thenlper/gte-base). This embedding model runs on the CPU. It should be able to run this on the GPU as well, but in my first tests it just slowed down everything (probably a case of not having enough VRAM).

4. Similarity Matching: When you ask a question, the app compares it with the text chunks and identifies the most semantically similar ones.

   - Before comparing the question with the text chunks, the app condenses the chat history and the question into a new question. That is done so that the question contains all the necessary information when being compared to the text chunks. See LangChain's documentation for [ConversationalRetrievalChain](https://api.python.langchain.com/en/latest/chains/langchain.chains.conversational_retrieval.base.ConversationalRetrievalChain.html).

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

2. Download an LLM compatible with [llama.cpp](https://github.com/ggerganov/llama.cpp), i.e. in [GGML format](https://github.com/rustformers/llm/tree/2f6ffd4435799ceaa1d1bcb5a8790e5b3e0c5663/crates/ggml). For testing purposes I used [
WizardLM-1.0-Uncensored-Llama2-13B-GGML](https://huggingface.co/TheBloke/WizardLM-1.0-Uncensored-Llama2-13B-GGML) ([wizardlm-1.0-uncensored-llama2-13b.ggmlv3.q2_K.bin](https://huggingface.co/TheBloke/WizardLM-1.0-Uncensored-Llama2-13B-GGML/blob/main/wizardlm-1.0-uncensored-llama2-13b.ggmlv3.q2_K.bin)) and put its path into `llm_params.json`. In the JSON file you can also specify other parameters, see LangChain's documenation for [LlamaCpp](https://api.python.langchain.com/en/latest/llms/langchain.llms.llamacpp.LlamaCpp.html). `n_gpu_layers` determines how much of the model shall be executed on the GPU, `n_gpu_layers=33` resulted in about 6 GB of VRAM being used:

   ```
   llama_model_load_internal: offloaded 33/43 layers to GPU
   llama_model_load_internal: total VRAM used: 4666 MB
   llama_new_context_with_model: kv self size  = 1600.00 MB
   ```

3. Run the `main.py` file using the Streamlit CLI. Execute the following command:
   ```
   streamlit run app.py
   ```

4. The application will launch in your default web browser, displaying the user interface.

5. Load multiple PDF documents into the app by following the provided instructions.

6. Ask questions in natural language about the loaded PDFs using the chat interface.
