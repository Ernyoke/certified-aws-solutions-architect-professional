# Amazon Polly

- Converts text in "life-like" speech
- The products takes text in specific languages and outputs speech in that specific language. Polly does not do translation!
- There 2 modes that Polly operates in:
    - Standard TTS:
        - Uses a concatenative architecture
        - Takes phonemes (smallest units of sound) to build patterns of speech
    - Neural TTS:
        - Takes phonemes, generate spectograms, it puts those spectograms through a vocoder form which gets the output audio
        - Much advanced way of generating human-like speech
- Output formats: MP3, Ogg Vorbis, PCM
- Polly is capable of using the Speech Synthesis Markup Language (SSML). This is a way we can provide additional control over how Polly generates speech. We can get Polly to emphasis certain part of the text or do certain pronunciation (whispering, Newscaster speaking style)