# Elastic Transcoder and AWS Elemental MediaConvert

- MediaConvert a fairly new AWS product replacing Elastic Transcoder
- MediaConvert is a superset of features provided by Transcoder
- Both of the systems are file based video transcoding systems
- They are serverless transcoding services for which we pay per use
- For both we add jobs to pipelines (ET) or queues (MC)
- Files are loaded from S3, processed and stored back to S3
- MC supports EventBridge for job signalling
- ET/MC architecture example:
    [ET/MC architecture](images/ElasticTranscoder&MediaConvert.png)
- We use these products when we need to the media convert in a serverless, event-driven media processing pipeline
- MC supports more codecs, design for larger volume and parallel processing
- MC supports reserved pricing
- ET required for WebM(VP8/VP9), animated GIF, MP3, Vorbis and WAV. For everything else we should use MediaConvert