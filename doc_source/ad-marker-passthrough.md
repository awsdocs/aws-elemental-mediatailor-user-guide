# Ad marker passthrough<a name="ad-marker-passthrough"></a>

By default for HLS, MediaTailor personalized manifests don't include the SCTE\-35 ad markers from the origin manifests\. When ad marker passthrough is enabled, MediaTailor passes through the following ad markers from origin manifests into personalized manifests:
+ EXT\-X\-CUE\-IN
+ EXT\-X\-CUE\-OUT
+ EXT\-X\-SPLICEPOINT\-SCTE35

 MediaTailor doesn't change the values for these markers\. For example, if `EXT-X-CUE-OUT` has a value of `60` in the origin manifest, but no ads are placed, MediaTailor won't change the value to `0` in the personalized manifest\. 

## Enable ad marker passthrough<a name="enable-ad-marker-passthrough"></a>

You can enable ad marker passthrough using the AWS Management Console or the AWS CLI\.

**To enable ad marker passthrough using the console**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1.  Select either **New Configuration** or **Edit Configuration**\.

1. In the **Advanced Settings** section, select **Enable** from the drop down menu\.

**To enable ad marker passthrough using the AWS CLI**  
Use the [put\-playback\-configuration](https://docs.aws.amazon.com/cli/latest/reference/mediatailor/put-playback-configuration.html) command\.