# Using package configurations<a name="channel-assembly-package-configurations"></a>

A package configuration is a representation of the source that contains the various packaging characteristics required for playback on different devices\. For example, you might have a source that has three packaged formats: HLS with DRM, DASH with segment timeline addressing, and HLS with CMAF segments\.

 Channel assembly doesn't repackage your sources\. If you want to include multiple packaged formats for a given source, you must make each packaged format available at the source location and specify the path to each packaged format\.

 Each package configuration object must include the following: 
+ **Relative path** \- The full path to the source's packaged format, relative to the source location\. For example, **/my/path/index\.m3u8**\.
+ **Source group** \- The name of the source group used to associate package configurations with a channel's output\.
+ **Type** \- Either HLS or DASH\.

 After you have created a channel, you must also declare each source group that you want to use for the channel's output\. 