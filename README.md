# VID-SNAP-AI
# VID SNAP AI

VID SNAP AI is a Flask-based web app that automatically converts a set of images and a short text description into an AI-narrated video reel. Upload your photos, write a caption, and the app generates a natural-sounding voiceover using the ElevenLabs text-to-speech API, then stitches the images and audio together into a polished vertical video reel using FFmpeg.

## Features

- **Web-based upload flow** — upload multiple images and a description through a simple browser interface
- **AI voiceover generation** — converts your description into natural speech using ElevenLabs' text-to-speech API
- **Automatic reel creation** — combines uploaded images and generated audio into a 1080x1920 (vertical/Reels-style) video using FFmpeg
- **Background processing queue** — a separate worker script continuously scans for new uploads and processes them into reels
- **Gallery view** — browse all generated reels in a built-in gallery page

## How It Works

1. The user uploads images and a text description through the `/create` page
2. Files are saved to a unique folder under `user_uploads/`, along with an `input.txt` (image sequence) and `desc.txt` (caption)
3. A background process (`generate_process.py`) watches for new upload folders
4. For each new folder, it:
   - Converts the description text to speech (`text_to_audio.py` via ElevenLabs)
   - Combines the images and audio into a final video reel using FFmpeg
5. Finished reels are saved to `static/reels/` and become viewable on the `/gallery` page

## Project Structure

```
VID SNAP AI/
├── main.py                 # Flask app: routes for home, create, and gallery
├── generate_process.py     # Background worker that builds reels from uploads
├── text_to_audio.py        # ElevenLabs text-to-speech integration
├── config.py                # API key configuration
├── templates/                # HTML templates (index, create, gallery, base)
├── static/
│   ├── reels/                # Generated video reels
│   ├── songs/                # Background music tracks
│   └── css/                  # Stylesheets
├── user_uploads/             # User-submitted images and descriptions
└── sample_images/            # Example images for testing
```

## Requirements

- Python 3.x
- [FFmpeg](https://ffmpeg.org/) installed and available on your system PATH
- An [ElevenLabs](https://elevenlabs.io/) API key

### Python dependencies

```
flask
werkzeug
elevenlabs
```

## Setup

1. Clone the repository
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Add your ElevenLabs API key in `config.py`:
   ```python
   ELEVENLABS_API_KEY = "your_api_key_here"
   ```
4. Run the Flask app:
   ```bash
   python main.py
   ```
5. In a separate terminal, start the background reel-generation worker:
   ```bash
   python generate_process.py
   ```
6. Open the app in your browser, upload images and a description on the **Create** page, then check the **Gallery** page once processing is complete.

## Notes

- `config.py` contains a sensitive API key — make sure to exclude it from version control (add it to `.gitignore`) before pushing to GitHub.
- The background worker (`generate_process.py`) must be running for uploads to be converted into reels.

## License

MIT License — feel free to fork, modify, and share.
