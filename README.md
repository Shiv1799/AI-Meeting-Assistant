# AI-Meeting-Assistant
This project is an AI-powered web application that processes audio recordings of meetings. It automatically transcribes the audio, intelligently corrects domain-specific financial terminology, and then generates concise meeting minutes and a list of actionable tasks.

The application is built using a "chain-of-thought" process involving multiple AI models: one for speech-to-text, a second for terminology correction, and a third for summarization and task extraction.

Application Screenshot
(Note: You will need to upload your image to your GitHub repository and name it demo_screenshot.png, or update the path in the Markdown above to match your image's file name and location.)

# Key Features
Simple Web Interface: Easily upload an audio file directly from your browser using a Gradio interface.

Automatic Speech Recognition: Transcribes spoken audio into text using OpenAI's Whisper model.

Intelligent Terminology Correction: A specialized prompt and LLM (Llama 3.2) are used to analyze the transcript and format financial terms correctly (e.g., "HSA" becomes "Health Savings Account (HSA)", "five two nine" becomes "529 (Education Savings Plan)").

Meeting Minutes & Task Generation: A second LLM (IBM Granite) processes the corrected transcript to generate structured meeting minutes and a clear, actionable task list.

# Downloadable Output: Provides the generated minutes and tasks as a .txt file for easy download and sharing.

# How it Works: The AI Pipeline
The application processes the user's audio file in a multi-step pipeline:

Audio Upload: The user uploads an audio file via the Gradio UI.

Transcription: The transcript_audio function uses the Hugging Face transformers pipeline with the openai/whisper-medium model to get a raw text transcript.

Text Cleaning: A simple helper function (remove_non_ascii) cleans the raw transcript.

Terminology Adjustment (LLM 1): The cleaned transcript is passed to the product_assistant function. This function sends the text to a meta-llama/llama-3-2-11b-vision-instruct model on IBM watsonx.ai with a specific system prompt, instructing it to find and format financial products and acronyms.

Minutes & Task Generation (LLM 2): The adjusted transcript (now with correct terminology) is passed into a LangChain. This chain uses a ChatPromptTemplate to structure the context for an ibm/granite-3-3-8b-instruct model (also on watsonx.ai), which generates the final meeting minutes and task list.

Display & Download: The final text is sent to the Gradio UI as both viewable text and a downloadable file.

# Tools & Technologies
Programming Language: Python

Web Framework:

gradio: Used to create and launch the simple, interactive web interface.

AI & LLM Services:

IBM watsonx.ai: The cloud platform used to host and serve the LLMs.

ibm-watsonx-ai: The official Python client library for watsonx.ai.

LangChain:

langchain: The core orchestration framework used to build the main generation pipeline.

langchain-ibm: Provides the WatsonxLLM integration for LangChain.

AI Models:

openai/whisper-medium: Used via the transformers library for high-quality automatic speech recognition.

meta-llama/llama-3-2-11b-vision-instruct: Used as the first LLM for the specialized task of correcting financial terminology.

ibm/granite-3-3-8b-instruct: Used as the second LLM for the final generation of meeting minutes and tasks.

# Core Libraries:

transformers: For running the Whisper ASR pipeline.

torch: As a backend dependency for the transformers pipeline.
