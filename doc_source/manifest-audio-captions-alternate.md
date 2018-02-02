# Alternate Audio Expected Behavior<a name="manifest-audio-captions-alternate"></a>

If your content contains alternate audio, AWS Elemental MediaTailor transcodes audio\-only renditions of the ads to the alternate audio tracks for your content\. This way, audio switching continues to work during ad breaks\. The service always inserts the default audio from the ad and replicates it across your audio tracks during ad breaks\.

The audio bit rate must be from 16 to 320 kHz for ad transcoding to succeed\.