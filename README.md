# midiLang
Create midi compositions with Large Language Models.

This app uses OpenAI LLMs to compose music. Then, music21 — a [Python Library](https://pypi.org/project/music21/) — creates a midi file from the LLM's composition. 

Midi looks like this in recording software: 

<br>

<img width="259" alt="Screenshot 2024-05-17 at 3 09 50 PM" src="https://github.com/kdlee17/midi_images/assets/139267850/94ace604-f6da-437d-a205-1ce3af00e829"><br>

<br>



With midi, notes aren't audio waves but rectangles that you can drag/delete/copy/paste to edit your composition easily. 

Now, let's build the app! 🤘

<br> 

## Installs

You need OpenAI and music21: 

```
pip install openai music21
```
<br>

## Music Recording Software
To use your midi files in a composition, you need recording software — a.k.a, a Digital Audio Workstation or DAW. 

If you don't have one, here's a few free options. Follow the links and download the software you prefer. 

**Mac**

• [Garageband](https://support.apple.com/garageband) - usually pre-installed on Apple computers  


**Mac and PC**

• [Waveform](https://www.tracktion.com/products/waveform-free) — free and user friendly

• [Reaper](https://www.reaper.fm/) - free for 60 days, then $60 <br>

<br>

## Clone the repository 

Next, clone this repository so you have the code you need to compose music with LLMs. Run this in your terminal: 

```
pip install git+https://github.com/username/repo.git
```

<br>

## Create an OpenAI API key

To use the app, you need an OpenAI subscription and API key. 

[Get a subscription](https://openai.com/api/).

Once you have a subscription, you can create an API key. Log into [OpenAI](https://platform.openai.com/apps). 

1. Log in and **click API**. 

2. Then, on the left side, go to **API Keys** > **Create New Key**. 


Now, let's add the key to your project. We'll save it as an environment variable. This means your script will use the key without the key being in your code, which keeps it safe. 🔒

Follow this [OpenAI quickstart doc](https://platform.openai.com/docs/quickstart) to set your API key as an environment variable and run a small script to test it! <br>

<br> 

## Make music with midiLang!

Now that setup is complete and your OpenAI API key is saved as an environment variable, you're ready to make music with OpenAI LLMs!

1. Open app.py
2. You can experiment with the system prompt and choose which model to call in the following function: <br>



```
def translate_to_notation(user_input):
    client = openai.Client(
        api_key=OPENAI_API_KEY 
    )
    chat_completion = client.chat.completions.create(
        messages=[
            {"role": "system", "content": "You are a music composer familiar with chord scales and music theory.\
            You compose music in ABC Notation. ONLY respond in ABC Notation. DONT FORGET TO SET NOTE LENGTH like this: \
            L:1/4  or L:1/8 or whichever length value works best. \
            Pay attention, ensure ALL chords are explicitly written as groups of notes in \
            square brackets for compatibility with music parsing libraries."}, # experiement with this prompt!
            {"role": "user", "content": user_input}
        ],
        model="gpt-3.5-turbo-0125" # change the model here!
    )

    print(chat_completion)
```
<br>


3. To compose music, adjust the user prompt in the main program flow: <br>



```
#main program flow

if __name__ == "__main__":
    user_input = "can you write a jazzy chord progression in D flat m?" # edit this prompt to compose music!
    abc_notation = translate_to_notation(user_input)
    if abc_notation:
        print("ABC Notation:", abc_notation)
        s = parse_abc_to_stream(abc_notation)
        if s is None:
            print("Parsing ABC notation failed. Please check the notation for errors.")
        else:
            convert_stream_to_midi(s, "output.mid") # change the file name here if you like
            print("midi file created:", "output.mid") # and make sure the name here matches the above name
    else:
        print("Failed to generate ABC notation.")
```
<br>

4. After you adjust the user prompt, run the program. Then, find the output.mid file (or whatever you choose to name the file) in your project directory. From there, drag the midi file into your recording software (DAW) and press play!

Have issues getting your midi file into your DAW? [Check out this video demo](https://www.youtube.com/watch?v=GyJKLqdg2fc&feature=youtu.be) or refer to your DAW's docs.  

<br>

> **Note:**
> Most DAWs create a new midi track automatically when you drag/drop a midi file into a project. You might see a prompt to "import tempo." Choosing "yes" updates the entire project to the tempo set by the LLM in the midi file. If you accept the tempo and don't like it, you can always re-adjust it manually. 
<br>


Happy composing! 🎸
