# AWS Elemental MediaTailor Support for DASH<a name="dash-support"></a>

AWS Elemental MediaTailor supports `.mpd` DASH manifests for live streaming as follows: 
+ Supports all DASH\-compliant manifests from MediaPackage\.
+ Follows the guidelines for DASH dynamic profile\.
+ Supports only SCTE\-35 2013 event streams and SCTE\-35 splice inserts\.
+ Supports segment templates that have segment timelines\. 
+ MediaTailor *does not* currently support the following: 
  + VOD 
  + Segment list, segment base, or segment templates with durations\. 