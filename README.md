# AI Voice Cover Generator

This project provides an end-to-end pipeline for generating AI voice covers of YouTube songs. It downloads the song's audio using `yt-dlp`, separates the vocals and instrumental tracks using Spleeter, and then applies a pre-trained AutoVC model (with a simple speaker embedding extraction as a placeholder) to convert the vocals to your own voice. Finally, the converted vocals are merged with the instrumental track using FFmpeg to produce a custom cover.

> **Note:**  
> - The AutoVC model loading and inference are provided as placeholders. You must supply your own pre-trained AutoVC checkpoint and, if needed, adapt the code to match your model's API.  
> - The speaker embedding extraction is currently a naive implementation (using the mean of the Mel-spectrogram). For production-quality conversion, use a proper speaker encoder.
> - This code is designed to run in Google Colab, though it can also be run locally on a compatible Python environment (preferably Python 3.8–3.10, since Spleeter may have issues with Python 3.11).

## Features

- **YouTube Audio Download:** Uses `yt-dlp` to download audio from a YouTube URL.
- **Vocal Separation:** Uses Spleeter (2-stems) to separate the vocals from the instrumental track.
- **Voice Conversion:** Converts the separated vocals to your voice using a pre-trained AutoVC model.
- **Audio Merging:** Uses FFmpeg to mix the converted vocals with the instrumental track, producing the final cover.

## Requirements

- Python (recommended: 3.8–3.10)
- [yt-dlp](https://github.com/yt-dlp/yt-dlp)
- [Spleeter](https://github.com/deezer/spleeter)
- [FFmpeg](https://ffmpeg.org/) (must be installed and available in your PATH)
- [Torch](https://pytorch.org/)
- [Librosa](https://librosa.org/)
- [SoundFile](https://pypi.org/project/SoundFile/)

## Installation

If using Google Colab, the provided cell automatically installs the required packages. For local installation, run:

```bash
pip install --upgrade pip setuptools wheel
pip install yt-dlp spleeter ffmpeg-python torch librosa soundfile
```

Also, make sure FFmpeg is installed on your system. For example, on Ubuntu:

```bash
sudo apt-get install ffmpeg
```

## Usage

1. **Upload your files:**  
   - Upload your pre-trained AutoVC checkpoint (e.g. `/content/autovc.ckpt`) to the working directory.  
   - Upload your voice sample (e.g. `/content/naat-wav.wav`).

2. **Run the Code:**  
   - In Google Colab, copy and paste the entire pipeline code (see below) into one cell and run it.
   - When prompted, enter:
     - The YouTube URL for the song.
     - The full path to your voice sample (e.g., `/content/naat-wav.wav`).

3. **Output:**  
   - The final AI-generated cover will be saved as `final_cover.mp3`.

## Code Overview

The pipeline consists of the following steps:

1. **Download YouTube Audio:**  
   The `download_youtube_audio()` function downloads the audio using `yt-dlp` and handles the potential double-extension issue.

2. **Separate Vocals and Instrumental:**  
   The `separate_vocals()` function uses Spleeter to separate the audio into vocals and accompaniment. It correctly locates the output folder based on the downloaded file name.

3. **Load Pre-trained AutoVC Model:**  
   The `load_autovc_model()` function loads your pre-trained AutoVC checkpoint. **(Customize this as needed)**

4. **Extract Speaker Embedding:**  
   The `extract_speaker_embedding()` function computes a naive speaker embedding from your voice sample.

5. **Voice Conversion (AutoVC Inference):**  
   The `autovc_inference()` function converts the separated vocals using the AutoVC model. The current implementation is a placeholder.

6. **Merge Audio Files:**  
   The `merge_audio_files()` function uses FFmpeg to merge the converted vocals with the instrumental track.

## Troubleshooting

- **Double Extension Issue:**  
  If you see filenames like `downloaded_song.mp3.mp3`, this is normal due to yt-dlp’s post-processing. The code checks for this and uses the correct file.

- **Spleeter Output Folder Not Found:**  
  The code dynamically constructs the expected output folder name based on the input file name. Ensure that the file name is handled correctly if you modify the output template.

- **Model Placeholder:**  
  The AutoVC conversion and speaker embedding extraction are placeholders. For real results, integrate your own model code and a robust speaker encoder.

## License

This project is provided under the MIT License. See the [LICENSE](LICENSE) file for details.
