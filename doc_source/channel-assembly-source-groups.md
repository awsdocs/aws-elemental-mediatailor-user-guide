# Using source groups with your channel's outputs<a name="channel-assembly-source-groups"></a>

A source group associates a package configuration with an output on a channel\. When you create the package configuration on the source, you identify the source group's name\. Then, when you create the output on the channel, you enter that same name to associate the output with the package configuration\. VOD sources that are added to a program on a channel must belong to the source group that's identified in the output\.

 For example:
+ VOD sources 1 and 2 both have three package configurations that have the source groups: **HLS**, **DASH**, and **HLS\-4k**\.
+ VOD source 3 has two package configurations with source groups **HLS** and **DASH**\.





 If channel A has two outputs with source groups **HLS** and **DASH**, the channel output can use all three VOD sources\. That's because VOD sources 1, 2, and 3 all have package configurations with source group labels **HLS** and **DASH**\.

If channel B has two outputs with source groups **HLS** and **HLS\-4k**, it can use VOD source 1 and 2, but not 3\. This is because VOD sources 1 and 2 both have package configurations with source group labels **HLS** and **HLS\-4k**\.

If channel C has a single output with the source group **DASH**, it can use all three VOD sources\. All three VOD sources have package configurations with the **DASH** source group\.