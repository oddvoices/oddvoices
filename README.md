# OddVoices

**OddVoices** is a project to create open-source singing synthesizers for General American English (GA). The goal is to create interesting voices for use in experimental music inspired by retro synthesizers. Expect quirky and unusual voices, and not necessarily intelligible ones.

Currently, OddVoices consists of two singing synthesizers:

- nt, a deep, dark basso profondo
- nwh, a barely intelligible synthesizer-like baritone

**This project is unstable and in an early stage of development.**

## Using the synthesizer

Install [Git LFS](https://git-lfs.github.com/) and make sure you are up to date:

    git lfs pull

Set up Python virtualenv:

    python -m venv .venv
    source .venv/bin/activate
    pip install -e .

Analyze segments in `voices/nt` directory and generate the database file at `nt.voice`. You only need to do this once:

    oddvoices-compile voices/nt nt.voice

Sing the JSON file at `example/music.json`:

    sing nt.voice example/music.json out.wav

Sing a MIDI file (experimental, very rudimentary right now):

    sing-midi nt.voice example/example.mid -l "This is just a test of singing" out.wav
    sing-midi nt.voice example/example.mid -f lyrics.txt out.wav

### Building and using the C++ synthesizer

The C++ synthesizer in the `liboddvoices` directory is a port of the Python synthesizer.

    cd liboddvoices
    mkdir build && cd build
    cmake ..

    liboddvoices_frontend ../../nwh.voice ../example/music.json out.wav

## Corpus

The corpus was created manually in the file `wordlist/words.txt`, which spans most common CV and CC diphones in GA.

Pronunciations are provided using X-SAMPA notation. X-SAMPA is similar to IPA, but uses ASCII characters. One minor change is that /æ/ is represented with `{}` to prevent bracket matching issues in text editors. (The closing curly bracket represents /ʉ/, which is not found in GA.)

## Recording a corpus

In a quiet recording environment, record all words in `wordlist/words.pdf` in order. I recommend using a pop filter if possible to prevent overly loud plosives. With 700 words and a few seconds each, you'll need to set aside about half an hour. To minimize fatigue, take breaks and record in multiple sessions.

All words should be sung in monotone without vibrato. Pick a note in a comfortable register where you can safely avoid cracks, vocal fries, and other artifacts, and keep that note fixed through the entire corpus. Poor intonation is not a big deal, but keep within 50 cents of the target frequency. It is fine to do multiple takes for a word.

Sing at a moderate pace and anunciate consonants well. Each syllable lasting about 2 seconds is good. For diphthongs, always sing with a long stable region and a short transition at the end. For example, the diphone `aU` should sound like a long "aah" followed by a short "ow."

Any word tagged "(long)" is an isolated vowel. Make these a few seconds long.

## Processing and tagging the corpus

Tagging a 30-minute corpus takes about two hours of work. If there are multiple audio files, concatenate them together into one big audio file. If there are multiple channels, take only one to ensure a mono audio file.

Open up the audio file in Audacity. (On Linux, be sure to use JACK and not ALSA or PulseAudio. Audacity lies about the endpoints during audio playback!) For each word, use Audacity's built-in labeling system to tag the diphones and vowels indicated by the light gray text.

For diphones, err on the side of longer: it is better for intelligibility to tag too long than too short. For diphthongs in diphones, tag the entire transition region. For vowels, tag only the stable region of diphthongs.
