# Provider Capabilities Comparison

## Music Providers

### 1. Google Lyria (lyria)
**Type:** Music Generation - Instrumental Only  
**API:** Google Gemini API  
**Status:** ✅ Available with GEMINI_API_KEY

**Capabilities:**
- Instrumental music generation
- Supports BPM control (default: 120)
- Duration: 10-120 seconds (default: 30)
- High-quality audio output
- Fast generation (typically < 1 minute)

**Strengths:**
- Free tier available through Google AI Studio
- Excellent audio quality
- Reliable and fast
- Good at following style prompts

**Limitations:**
- ❌ No vocal/lyrics support
- ❌ Instrumental only
- Limited control over specific instruments

**Best For:** Background music, instrumentals, ambient scores, cinematic tracks

**Example Usage:**
```bash
uv run ai-content music --style jazz --provider lyria --duration 30
uv run ai-content music --prompt "Upbeat electronic dance music" --provider lyria --bpm 128
```

---

### 2. MiniMax Music (minimax)
**Type:** Music Generation - WITH Vocals/Lyrics  
**API:** AIMLAPI  
**Status:** ✅ Available with AIMLAPI_KEY

**Capabilities:**
- Music generation with vocals and lyrics
- Supports custom lyrics from `.txt` files
- Reference audio URL for style transfer
- Async generation (takes 2-5 minutes)
- Job tracking with status polling

**Strengths:**
- ✅ Supports vocals and lyrics (UNIQUE FEATURE)
- Can generate songs with custom lyrics
- Style transfer from reference audio
- Professional vocal quality

**Limitations:**
- Slower generation time (async API)
- Requires AIMLAPI credits (paid service)
- More complex workflow (job ID tracking)

**Best For:** Songs with lyrics, vocal tracks, complete musical compositions

**Example Usage:**
```bash
# With lyrics file
uv run ai-content music --prompt "Upbeat pop song" --provider minimax --lyrics lyrics.txt

# Check status and download
uv run ai-content music-status --job-id <id> --download
```

---

## Video Providers

### 3. Google Veo 2 (veo)
**Type:** Video Generation  
**API:** Google Gemini API  
**Status:** ✅ Available with GEMINI_API_KEY

**Capabilities:**
- Text-to-video generation
- Image-to-video (first frame conditioning)
- Duration: 5-10 seconds
- Aspect ratios: 16:9, 9:16, 1:1, 21:9
- High-quality cinematic output
- Camera movement control

**Strengths:**
- Excellent video quality
- Good at cinematic prompts
- Supports multiple aspect ratios
- Image-to-video capability
- Fast generation

**Limitations:**
- Short duration (5-10 seconds max)
- Expensive credits on free tier
- Limited animation control

**Best For:** Cinematic shorts, music video clips, establishing shots, title sequences

**Example Usage:**
```bash
uv run ai-content video --style nature --provider veo --duration 5
uv run ai-content video --prompt "A dragon flying over mountains" --provider veo --aspect 21:9
uv run ai-content video --image first_frame.png --prompt "Camera pans right" --provider veo
```

---

### 4. KlingAI (kling)
**Type:** Video Generation  
**API:** KlingAI Direct API  
**Status:** ⚠️ Requires separate KLING_API_KEY (not needed for this challenge)

**Capabilities:**
- Text-to-video generation
- Professional quality output
- Longer duration support

**Note:** This provider is optional and not required for the challenge. Focus on Veo for video generation.

---

## Image Providers

### 5. Google Imagen 3 (imagen)
**Type:** Image Generation  
**API:** Google Gemini API  
**Status:** ✅ Available with GEMINI_API_KEY

**Capabilities:**
- Text-to-image generation
- High-resolution output
- Photorealistic or artistic styles
- Fast generation

**Best For:** First frames for image-to-video, album covers, thumbnails

**Example Usage:**
```bash
uv run ai-content image --prompt "Majestic lion in savanna" --provider imagen
```

---

## Provider Selection Guide

### For Music WITHOUT Vocals → Use **Lyria**
- Jazz, blues, cinematic, electronic, ambient
- Background music for videos
- Instrumental soundtracks

### For Music WITH Vocals/Lyrics → Use **MiniMax**
- Songs with custom lyrics
- Vocal performances
- Complete musical compositions

### For Video → Use **Veo**
- Cinematic video clips
- Nature, urban, fantasy scenes
- Music video visuals

### For Images → Use **Imagen**
- First frames for video
- Album artwork
- Thumbnails

---

## API Key Requirements Summary

| Provider | API Key Required | How to Get | Free Tier? |
|----------|-----------------|------------|------------|
| Lyria | `GEMINI_API_KEY` | [aistudio.google.com](https://aistudio.google.com/) | ✅ Yes |
| Veo | `GEMINI_API_KEY` | [aistudio.google.com](https://aistudio.google.com/) | ✅ Yes (limited) |
| Imagen | `GEMINI_API_KEY` | [aistudio.google.com](https://aistudio.google.com/) | ✅ Yes |
| MiniMax | `AIMLAPI_KEY` | [aimlapi.com](https://aimlapi.com/) | ⚠️ Paid credits |
| Kling | `KLING_API_KEY` | [klingai.com](https://klingai.com/) | ⚠️ Optional |

**Minimum Required for Challenge:** Just `GEMINI_API_KEY` (covers music + video + images)
