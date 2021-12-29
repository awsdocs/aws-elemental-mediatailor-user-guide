# Document history for AWS Elemental MediaTailor<a name="document-history"></a>

The following table describes important changes to this documentation\. 

| Change | Description | Date | 
| --- |--- |--- |
| [New IAM managed policy policy topic](security-iam-awsmanpol.md) | Added two new managed policies for MediaTailor\. | November 24, 2021 | 
| [New `AWSElementalMediaTailorReadOnly` managed policy](security-iam-awsmanpol.md#security-iam-awsmanpol-updates) | Added a new AWS managed policy that grants permissions that allows read\-only access to MediaTailor resources\. | November 10, 2021 | 
| [New `AWSElementalMediaTailorFullAccess` managed policy](security-iam-awsmanpol.md#security-iam-awsmanpol-updates) | Added a new AWS managed policy that allows full access to MediaTailor resources\. | November 10, 2021 | 
| [New confused deputy topic](cross-service-confused-deputy-prevention.md) | Added a topic that explains how to prevent the confused deputy problem\. | November 4, 2021 | 
| [Prefetching ads topic](prefetching-ads.md) | MediaTailor can now prefetch ads for ad breaks before they occur\. | October 12, 2021 | 
| [Added logging configuration settings for playback configurations](configurations-create.md#configurations-create-addl) | Use logging configuration settings to control settings related to playback configuration logs\. | September 28, 2021 | 
| [New linear playback mode](channel-assembly-creating-channels.md) | Added a new linear playback mode\. | September 1, 2021 | 
| [New absolute transition type](channel-assembly-adding-programs.md) | Added support for absolute transition types, which allow you to set a wall clock start time for your program on linear channels\. | September 1, 2021 | 
| [New channel assembly alerts topic](channel-assembly-alerts.md) | You can now monitor your channel assembly resources using MediaTailor alerts\. When an issue or a potential issue occurs with your channel assembly resources, MediaTailor generates alerts\. | July 14, 2021 | 
| [New source location authentication type](channel-assembly-source-locations-access-configuration.md) | MediaTailor now supports Secrets Manager access token authentication\. | June 16, 2021 | 
| [Support for additional UPID types](variables-session.md) | MediaTailor now supports ADS Information \(0xE\) and User Defined \(0x1\) segmentation UPID types\. | April 15, 2021 | 
| [New segmentation UPID dynamic variables](variables-session.md) | There are three new dynamic variables: `scte.segmentation_upid.assetId`, `scte.segmentation_upid.cueData.key`, and `scte.segmentation_upid.cueData.value`\. These variables are used in conjunction with the MPU segmentation UPID type \(0xC\) for podbuster workflows\. | April 15, 2021 | 
| [New channel assembly service description](what-is.md) | Added information about the new channel assembly service\. | March 11, 2021 | 
| [New MediaTailor channel assembly service documentation](channel-assembly.md) | Channel assembly is a new manifest\-only service that allows you to create linear streaming channels using your existing video on demand \(VOD\) content\. | March 11, 2021 | 
| [Added channel assembly quotas](quotas.md) | Added quotas for the new MediaTailor channel assembly service\. | March 11, 2021 | 
| [New channel assembly terms](what-is-terms.md) | Added terms that correspond to the new channel assembly service\. | March 10, 2021 | 
| [Channel assembly tagging support](tagging.md) | Added support for tagging of channel assembly resources in AWS Elemental MediaTailor\. Channels, SourceLocations, and VodSources support tagging\. | March 9, 2021 | 
| [New dynamic variables topic](variables-domains.md) | MediaTailor now supports dynamic domain variables\. | February 25, 2021 | 
| [Added optional configuration alias settings](configurations-create.md#configurations-create-addl) | Use configuration aliases alongside domain variables to dynamically configure domains during session initialization\. | February 25, 2021 | 
| [New `scte.segmentation_upid` dynamic ad variable](variables-session.md) | Added support for the `scte.segmentation_upid` session data dynamic ad variable\. | December 5, 2020 | 
| [New ad marker passthrough topic](ad-marker-passthrough.md) | Ad marker passthrough is now available for HLS manifests\. | October 29, 2020 | 
| [Updated configuration advanced settings](configurations-create.md#configurations-create-addl) | Ad marker passthrough is a new playback configuration advanced setting\. | October 14, 2020 | 
| [New debug log mode](debug-log-mode.md) | New topic on the DEBUG log mode\. | August 14, 2020 | 
| [Clarification around EXT\-X\-CUE\-OUT duration attribute for bumpers](bumpers.md) | Updated the bumpers requirements so that for HLS the `duration` attribute is required for each `EXT-X-CUE-OUT` tag\. | August 5, 2020 | 
| [New bumpers topic](bumpers.md) | Added a new bumpers topic | July 27, 2020 | 
| [Ad Suppression is available for DASH](ad-suppression.md) | Ad suppression is now available for DASH\. Removed the "HLS\-only" restriction from the ad suppression topic\. | June 3, 2020 | 
| [Update console\-specific names](configurations-create.md#configurations-create-addl) | Updated console\-specific names to reflect a newer version of the console UI\. | May 1, 2020 | 
| [New `avail.index` dynamic ad variable](variables-session.md) | Added support for the new `avail.index` session data dynamic ad variable\. | March 13, 2020 | 
| [New `AdVerifications` and `Extensions` elements](ad-reporting-client-side.md) | For client\-side reporting, the `AdVerifications` and `Extensions` elements are supported\. | March 10, 2020 | 
| [Personalization threshold configuration](configurations-create.md#configurations-create-addl) | Added support for the personalization threshold optional configuration\. | February 14, 2020 | 
| [DASH VOD manifests](manifest-dash.md) | Added support for video on demand \(VOD\) DASH manifests from the origin server, with multi\-period manifest output\. | December 23, 2019 | 
| [Console support for transcode profile name](configurations-create.md) | Added description for transcode profile name in the configuration\. | December 23, 2019 | 
| [Updated limits tables](quotas.md) | Updated limits for ADS redirects and ADS timeouts\. | December 18, 2019 | 
| [CDN best practices](cdn-bp.md) | Added a section about content distribution network \(CDN\) best practices for personalized manifests\.  | December 13, 2019 | 
| [Document live pre\-roll behaviors](ad-behavior-preroll.md) | Added *Pre\-Roll Ad Insertion* section to describe how live pre\-roll ads work with AWS Elemental MediaTailor\. | November 26, 2019 | 
| [Support for live pre\-roll ads](configurations-create.md) | Added support for inserting pre\-roll ads at the beginning of a live stream\. | September 11, 2019 | 
| [Analyzing ADS logs in Amazon CloudWatch Logs insights](monitor-cloudwatch-ads-logs.md) | Added information for using the AWS Elemental MediaTailor ADS logs and CloudWatch Logs Insights to analyze your MediaTailor sessions\. | August 13, 2019 | 
| [New security chapter](security.md) | Added a security chapter to enhance and standardize coverage\. | May 23, 2019 | 
| [DASH single\-period manifests](manifest-dash.md) | Added support for single\-period DASH manifests from the origin server, with multi\-period manifest output\. | April 4, 2019 | 
| [Support for SCTE\-35 UPIDs in the ADS URL](variables-session.md) | Added support for including a unique program ID \(UPID\) in the ad decision server \(ADS\) URL\. This allows the ADS to provide program\-level ad targeting within a live linear stream\.  | March 28, 2019 | 
| [Client\-side reporting supports companion ads](ad-reporting-client-side.md) | For client\-side reporting, the AWS Elemental MediaTailor tracking URL response now includes companion ad metadata\.  | March 28, 2019 | 
| [HLS ad marker documentation](hls-ad-markers.md) | Added a section that describes supported HLS ad markers\. | March 1, 2019 | 
| [Tagging support](tagging.md) | Added support for tagging of configuration resources in AWS Elemental MediaTailor\. Tagging allows you to identify and organize your AWS resources and control access to them and to track your AWS costs\. | February 14, 2019 | 
| [Added AWS CloudTrail logging information](logging-using-cloudtrail.md) | Added topic about using CloudTrail to log actions in the AWS Elemental MediaTailor API\. | February 11, 2019 | 
| [Added section on playback errors](playback-errors.md) | Added information about the errors that might be returned by MediaTailor during playback, in response to requests from a player or a content delivery network \(CDN\)\. | February 4, 2019 | 
| [DASH base64\-encoded binary](manifest-dash.md) | Added support for providing splicing information in manifests in base64\-encoded binary, inside `<scte35:Signal>` `<scte35:Binary>` markers\. | January 4, 2019 | 
| [DASH time signal](manifest-dash.md) | Added support for providing splicing information in manifests inside `<scte35:TimeSignal>` markers\. | December 5, 2018 | 
| [DASH location support](dash-location-feature.md) | Added support for the MPEG\-DASH `<Location>` tag\. | December 4, 2018 | 
| [DASH support](manifest-dash.md) | Added support for MPEG\-DASH manifests\. | November 14, 2018 | 
| [Updated limits tables](quotas.md) | Updated limits for configurations and manifest size\. | October 13, 2018 | 
| [New and updated metrics](monitoring-cloudwatch-metrics.md) | Added metrics for ad decision server \(ADS\) and origin timeouts, and updated the ADS and origin error definitions to include timed\-out responses\.  | October 13, 2018 | 
| [Better documentation coverage for server\-side and client\-side ad insertion use cases](variables.md) | Expanded description and examples to cover the use of dynamic ad variables for server\-side ad insertion and for client\-side ad insertion\.  | October 1, 2018 | 
| [New regions](what-is.md#regions-endpoints) | Added support for the PDX and FRA Regions\. | July 18, 2018 | 
| [VAST/VPAID](vast.md) | Added information about VAST and VPAID\.  | March 16, 2018 | 
| [CloudWatch](monitoring.md) | Added information about available CloudWatch metrics, namespaces, and dimensions\.  | March 16, 2018 | 
| [New regions](what-is.md#regions-endpoints) | Added support for the Asia Pacific \(Singapore\), Asia Pacific \(Sydney\), and Asia Pacific \(Tokyo\) Regions\. | February 8, 2018 | 
| [Default Amazon CloudFront distribution paths](integrating-cdn-standard.md) | Added the list of paths for the Amazon CloudFront distribution where AWS Elemental MediaTailor stores ads\.  | February 6, 2018 | 
| [IAM policy information](setting-up.md) | Added IAM policy information specific to AWS Elemental MediaTailor\. Added instructions for creating non\-admin roles with limited permissions\.  | January 3, 2018 | 
| [First release](what-is.md) | First release of this documentation\. | November 27, 2017 | 

**Note**  
The AWS Media Services are not designed or intended for use with applications or in situations requiring fail‐safe performance, such as life safety operations, navigation or communication systems, air traffic control, or life support machines in which the unavailability, interruption or failure of the services could lead to death, personal injury, property damage or environmental damage\.