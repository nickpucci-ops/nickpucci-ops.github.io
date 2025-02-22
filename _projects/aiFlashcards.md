---
title: "AI Flashcard Generator"
excerpt: "An automatic flashcard generator featuring Meta's Llama 3 LLM"
date: 2025-02-21
collection: projects
layout: single
header:
  image: /assets/images/flashcard-gen/flashcard4.png
  teaser: /assets/images/flashcard-gen/flashcard4.png
toc: true
toc_label: "Table of Contents"
sidebar:
  - title: "Tools Used"
    text: "Python, GPT4All, Llama 3, Tkinter"
gallery:
  - url: /assets/images//flashcard-gen/flashcard4.png
    image_path: /assets/images/flashcard-gen/flashcard4.png
    alt: "Tkinter display"
---

# Introduction
AI Flashcard Generator is a Python-based application that automatically generates concise, AI-powered definitions for any topic. This tool is built using Llama-3-8B-Instruct and the GPT4All framework with a simple Tkinter-based graphical user interface.

# Features
- **Concise AI-generated definitions of any topic**
- **Custom GUI**
- **Offline Functionality**
- **(Plan for future updates to include saving and exporting flashcard sets)**

# Gallery
{% include gallery class = "full" %} 

# Link to Github
You can check out the source code here => [github](https://github.com/nickpucci-ops/AI-flashcard-generator)  
To run this, you will need to have GPT4All, Tkinter, and the Llama-3-8B-Instruct model installed in your local env

# Development Process
## Idea & Planning
The goal was to create an AI-powered study tool that generates flashcards on demand, mainly for practice and offline functionality is pretty cool. I wanted to explore local LLMs and experiment with prompt engineering while also improving my knowledge of GUI development with Tkinter. 

## Implementation
The core mechanics were built using Python and utilizing GPT4All's toolkit. GPT4All offers free LLM models for download, which is a perfect alternative to buying an API key from an online chatbot. I chose Llama 3 8B Instruct because it had fairly lightweight hardware requirements compared to the others. Meta also provides online documentation about Llama 3, as they are instruction-tuned models optimized for dialogue use cases and outperform many of the available open-source chat models on common industry benchmarks.

## Challenges & Solutions
When I first started this project, my understanding of prompt engineering was limited, which led to inconsistent and unpredictable outputs. The model would sometimes format responses strangely or even generate conversations with itself.  
![flashcard1.png](/assets/images/flashcard-gen/flashcard1.png)

After experimenting with different approaches and realizing my initial method wasn’t working, I researched how others structured system prompts for LLMs. That’s when I came across Meta’s Llama 3 documentation, which provided insights into its model card and instruction syntax. Applying these best practices drastically improved output consistency. The model then could generate clean and concise definitions as intended.

On launch, I encountered errors related to missing CUDA libraries, despite the model running fine. These errors occurred because GPT4All was attempting to use Nvidia’s CUDA for GPU acceleration, but since I was running the model on a CPU-only laptop, it defaulted to CPU processing. While this meant slightly slower response times (several seconds per request), it didn’t break functionality, which I was ok with because this allowed me to add an epic loading animation with Python multithreading.

The next plan is to find a way to implement a save feature so that flashcards can be stored in sets and exported for later use or sharing. I'm also going to use this project as a practice tool to work on front-end development and better user interaction.

---
