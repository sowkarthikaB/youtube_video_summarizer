# youtube_video_summarizer
This notebook transcribes after getting the URL of the youtube video and later adds punctuation and summarizes the video. The punctuation and summarization is done with GROK with the help of API keys

A simple Python pipeline that fetches a YouTube video transcript, adds punctuation, and summarizes it using a free LLM API.

## Features

- Fetches transcript from any YouTube video
- Restores punctuation using LLaMA
- Summarizes the transcript into key points
- Free to use via Groq API

## Requirements

- Python 3.x
- `youtube-transcript-api >= 0.6.0`
- `groq`

## Installation and Setup

```bash
pip install youtube-transcript-api
```

1. Get a free API key from [console.groq.com](https://console.groq.com)
2. Replace `your_groq_api_key` in the code with your actual key

## Usage

### Step 1: Fetch Transcript

```python
from youtube_transcript_api import YouTubeTranscriptApi

url = 'https://www.youtube.com/watch?v=nHTyRkzVZCU'
video_id = url.split('v=')[1]
transcript = YouTubeTranscriptApi().fetch(video_id)
text = ' '.join([t.text for t in transcript])
print(text)
```

### Step 2: Punctuate and Summarize

```python
from groq import Groq

client = Groq(api_key="your_groq_api_key")

# Step 1: Add punctuation
response1 = client.chat.completions.create(
    model="llama-3.1-8b-instant",
    messages=[{
        "role": "user",
        "content": f"Add proper punctuation and capitalization to this transcript. Return only the corrected text:\n\n{text}"
    }]
)
punctuated_text = response1.choices[0].message.content

# Step 2: Summarize
response2 = client.chat.completions.create(
    model="llama-3.1-8b-instant",
    messages=[{
        "role": "user",
        "content": f"Summarize the text:\n\n{punctuated_text}"
    }]
)
summary = response2.choices[0].message.content
print(summary)
```

## How It Works

1. **Fetch** — `youtube-transcript-api` pulls the raw transcript from the YouTube video
2. **Punctuate** — Sends raw text to LLaMA via Groq to restore punctuation and capitalization
3. **Summarize** — Sends the punctuated text to LLaMA again for a clean summary

## Notes

- Keep your API key private — do not share or commit it to GitHub
- Groq free tier has rate limits; avoid running in large loops
- Only works on videos that have transcripts/subtitles enabled
- Works with Python 3.13 (avoids compatibility issues with local NLP libraries)

  <img width="1920" height="1008" alt="image" src="https://github.com/user-attachments/assets/a519856a-fa16-4159-a016-83e8404bd889" />

  ## Final Output image
  <img width="1920" height="1008" alt="image" src="https://github.com/user-attachments/assets/600841d5-1191-44b4-b8c4-64dd2bd122c1" />





