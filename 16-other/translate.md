# Amazon Translate

- Is a text translation service based in ML
- Translates text from native language to other languages one word at a time
- Translation process has two parts:
    - Encoder reads the source text => outputs a semantic representation (meaning)
    - Decoder reads in the meaning => writes to the target language
- Attention mechanism ensures "meaning" is translated
- Textract is capable to detect the source text language
- Use cases:
    - Multilingual user experience
    - Translate incoming data (social media/news/communications)
    - Language-independence for other AWS services: we might have other services such as Comprehend, Transcribe and Polly which operate on information; Transcribe will make this services operate in a language independent way. It can used to analyze data stored in S3, RDS, DDB, etc.
    - Commonly used for integration with other services/Apps/platforms