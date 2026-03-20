
## DS 246 - Generative and Agentic AI in Practice

# Real-Time Agentic Voice Assistant with Web Search & Low Latency

This real-time **agentic voice assistant** provides a truly conversational experience with intelligent web search capabilities. The system autonomously decides when to search the web, gathers context from multiple sources, and delivers informed responses with proper citations - all while maintaining minimal latency for seamless interaction.

## 🎯 Core Features

### 1:            **Voice Activity Detection (VAD)**
We utilize an advanced VAD module to intelligently detect when a user begins speaking, activating the recording process only when needed. This ensures:
- No background noise recording
- Efficient resource usage
- Natural conversation flow

### 2: **Speech-to-Text (FasterWhisper)**
Once the user finishes speaking, recorded audio is immediately transcribed using **FasterWhisper (medium model)** running on CUDA for:
- Fast and accurate speech-to-text conversion
- Multi-language support
- Low-latency processing

### 3: **Agentic Query Processing**
The system acts as an **intelligent agent** that:
- Analyzes user intent to determine if web search is needed
- Refines conversational queries into optimized search terms
- Uses **Gemma 3 (4B) LLM** to transform natural language into effective search queries

**Example:**
```
User: "i have my vehicle's engine light on what to do and how to fix it"
Agent: "How to diagnose and fix vehicle engine light" (refined search)
```

### 4: **Web Search & Content Extraction**
When web search is needed, the system:
- **Searches** via Serper API (Google Search API)
- **Classifies** links (web pages, videos, social media, forums)
- **Scrapes** relevant web pages using Trafilatura
- **Extracts** clean, readable content from multiple sources in parallel
- **Filters** promotional content and focuses on informative sources

Supported content types:
- ✅ Web articles and blogs
- ✅ Technical documentation
- ❌ Videos (detected but skipped for text extraction)
- ❌ Social media (filtered for quality)

### 5: **Context-Aware Response Generation**
The transcribed text + gathered web context is fed into **Gemma 3 Large Language Model**, which:
- Processes the user query with real-time web information
- Streams responses naturally as they're generated
- Cites sources with proper attribution
- Formats responses in clean Markdown with:
  - Step-by-step explanations
  - Source links
  - Recommended resources

### 6: **Streaming Text-to-Speech (Coqui-XTTSv2)**
As Gemma 3 streams its response:
- Text is segmented based on punctuation (sentences)
- Each complete sentence is passed to **Coqui-XTTSv2 TTS model**
- Audio is generated with natural voice cloning

### 7: **Concurrent Audio Streaming**
We employ a sophisticated concurrent audio pipeline:
- Generated audio segments are added to an **async audio queue**
- An **audio worker** operates in parallel, continuously streaming
- Ensures continuous audio flow while processing new sentences
- Maintains low latency between LLM output and audio playback

### 8: **Smart Voice Interruption**
Critical **interrupt feature** with VAD-based detection:
- If user begins speaking during TTS playback, system immediately recognizes it
- Stops all ongoing processes (LLM streaming, TTS generation, audio playback)
- Clears audio queue and promptly returns to listening mode
- Prioritizes user input over system output

### 9: **Seamless Conversation Loop**
Once the audio queue is empty:
- System seamlessly transitions back to listening mode
- Ready for the next interaction
- Maintains conversation context and history


---

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Speech Recognition** | FasterWhisper (Medium) + CUDA | Fast, accurate transcription |
| **Voice Activity Detection** | WebRTC VAD | Detect speech start/end |
| **Large Language Model** | Gemma 3 (4B) via Ollama | Query refinement & response generation |
| **Web Search** | Serper API | Google search results |
| **Web Scraping** | Trafilatura | Clean content extraction |
| **Text-to-Speech** | Coqui-XTTSv2 | Natural voice synthesis |
| **Async Processing** | Python asyncio | Concurrent operations |
| **Audio Handling** | sounddevice + NumPy | Real-time audio I/O |

---

## 📊 System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        USER SPEAKS                          │
└────────────────────┬────────────────────────────────────────┘
                     ↓
              [VAD Detection]
                     ↓
          [Audio Recording Buffer]
                     ↓
          [FasterWhisper Transcription]
                     ↓                       
             [Web Search]          
                     ↓
             [Serper API]
                     ↓
             [Parallel Web Scraping]
                     ↓
             [Content Extraction]
                     ↓
             [Context Assembly]
                     ↓
          [Gemma 3 LLM Processing]
                     ↓
         [Streaming Text Response]
                     ↓
           [Sentence Segmentation]
                     ↓
          [TTS Audio Generation]
                     ↓
           [Audio Queue Worker]
                     ↓
       ┌─────────────┴──────────────┐
       ↓                            ↓
[Audio Playback]          [VAD Interrupt Monitor]
       ↓                            ↓
   [Complete]                 [User Speaks?]
       ↓                            ↓
       └────────────┬───────────────┘
                    ↓
           [Return to Listening]
```

---

## 🚀 Key Advantages

✅ **Real-time web access** - Always up-to-date information  
✅ **Source attribution** - Cites where information came from  
✅ **Low latency** - Concurrent processing minimizes delays  
✅ **Natural interruption** - User can speak anytime to interrupt  
✅ **Intelligent search** - Only searches when actually needed  
✅ **Quality filtering** - Avoids promotional and low-quality content  
✅ **Markdown formatting** - Clean, structured responses  
✅ **Voice cloning** - Natural-sounding personalized TTS  
✅ **GPU acceleration** - CUDA support for speed  

---

## 📈 Performance Characteristics

- **Transcription Latency**: ~1-2 seconds (FasterWhisper on CUDA)
- **Search + Scraping**: ~2-4 seconds (parallel processing)
- **LLM Response Start**: ~0.5-1 second (streaming)
- **TTS Generation**: ~0.3-0.8 seconds per sentence
- **Total Response Time**: 3-8 seconds (with web search), <2 seconds (direct response)
- **Interrupt Response**: <500ms

---

## 🎓 Use Cases

- **Technical Support**: "My laptop won't boot, help me fix it"
- **Learning**: "Explain quantum computing in simple terms"
- **Troubleshooting**: "Why is my car making a clicking sound?"
- **Research**: "What are the latest developments in AI?"
- **How-to Guides**: "How do I bake sourdough bread?"
- **Product Recommendations**: "Best budget laptops for programming"

---


















































# VoiceAssistant
