# Audio Transcription using OpenAI API

This Python script uses the OpenAI API to transcribe audio files. It takes a directory of audio files as input, transcribes them, and outputs the transcriptions into text files in a specified output directory.

```python
import os
import openai
import time

# Prompt for OpenAI API key
openai.api_key = input("Please enter your OpenAI API key: ")

# Prompt for the input and output directories
audio_dir = input("Please enter the path to the input directory: ")
output_dir = input("Please enter the path to the output directory: ")

# Create a log file
log_file = open("transcription_log.txt", "w")

# Loop through all the audio files
for audio_file in os.listdir(audio_dir):
    if audio_file.endswith(('.mp3', '.m4a', '.wav')):  # Check that it's an audio file
        # Full path of the current audio file
        audio_path = os.path.join(audio_dir, audio_file)

        # Prepare the output file name and path
        output_file_name = os.path.splitext(audio_file)[0] + ".txt"  # Get the base name of the audio file
        output_path = os.path.join(output_dir, output_file_name)  # Full path of the output text file

        # Check if this file has already been processed
        if os.path.exists(output_path):
            msg = f'Skipped: {audio_file} has already been transcribed.'
            print(msg)
            log_file.write(msg + '\n')
            continue

        print(f'Starting transcription for {audio_file}...')
        
        try:
            # Read audio file
            with open(audio_path, "rb") as file:
                # Transcribe the audio file
                transcript = openai.Audio.transcribe(
                    file=file,
                    model="whisper-1",
                    response_format="text"
                )

            # Save the transcript to a .txt file
            with open(output_path, "w", encoding="utf-8") as output_file:
                output_file.write(transcript)

            msg = f'Success: Transcribed {audio_file}'
            print(msg)
            log_file.write(msg + '\n')
        except Exception as e:
            msg = f'Error: Could not transcribe {audio_file}. Reason: {str(e)}'
            print(msg)
            log_file.write(msg + '\n')
        
        # Wait for 10 seconds before proceeding with the next file
        print("Waiting for 10 seconds...")
        time.sleep(10)

log_file.close()
print("Transcription completed for all audio files.")

```

This script uses the OpenAI API to convert speech to text. It goes through each audio file in the input directory, transcribes it using the OpenAI API, and writes the transcription to a text file in the output directory.

## Usage
Run the script.
When prompted, enter your OpenAI API key.
Enter the path to the input directory (where your audio files are located).
Enter the path to the output directory (where the transcriptions will be saved).
The script will transcribe each audio file, and print progress messages to the console.

## Requirements
You need to have the os, openai, and time Python modules installed to run this script. You also need to have a valid OpenAI API key.


