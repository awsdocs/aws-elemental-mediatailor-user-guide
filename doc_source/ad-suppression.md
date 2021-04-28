# Configuring ad break suppression<a name="ad-suppression"></a>

 * Ad suppression is only available for live workflows\. * 

You can configure MediaTailor to skip ad break personalization for live content\. This is known as *ad suppression*, or *avail suppression*\.

Ad suppression can be used for the following use cases\.
+ **Large manifest lookback window** – If a viewer starts playback at the live edge of a manifest but the lookback window is large, you might want to only insert ads starting after the viewer started watching\. Or, insert ads for a portion of the total lookback window in the manifest\. You can configure ad suppression so that MediaTailor personalizes ad breaks on or within a specified time range behind the live edge\.
+ **Mid\-break join** – If the viewer starts watching a live video stream in the middle of an ad break, that user is likely to change the channel and not watch the advertisement\. Ad suppression enables you to skip ad break personalization if the ad break started before the viewer joined the stream\.

## Configuring ad suppression<a name="working-with-ad-suppression"></a>

To use ad suppression, you configure an **avail suppression mode** and **avail suppression value** in the MediaTailor console, AWS CLI, MediaTailor API, or as parameters in your client's playback session request\. For information about configuration via parameters, see [Configuring ad suppression parameters – playback session request](#configuring-ad-suppression-parameters-playback-session-request)\.

The following are the ad suppression configuration parameters:
+ **Avail suppression mode** – Sets the ad suppression mode\. By default, ad suppression is off\. **Accepted values**: `OFF` or `BEHIND_LIVE_EDGE`\. If the mode is set to `BEHIND_LIVE_EDGE` MediaTailor, doesn't personalize ad breaks that start before the live edge minus the **avail suppression value**\.
+ **Avail suppression value** – A time relative to the live edge in a live stream\. **Accepted value**: A time value in `HH:MM:SS`\.

The following are examples of ad suppression settings\.

**Example 1: No ad suppression**  
When the **avail suppression mode** is `OFF` there's no ad suppression and all ad breaks are personalized\.  

![\[In the center of the image is a live content stream interspersed with three ad breaks, above a timeline from start to 1+ hours. Each ad break says ad break personalization. Bisecting the third ad break is a vertical dotted green line that represents the live edge.\]](http://docs.aws.amazon.com/mediatailor/latest/ug/images/no_ad_suppression.png)

**Example 2: Value in sync with live edge**  
When the **avail suppression value** is set to `00:00:00`, it is in sync with the live edge\. MediaTailor won't personalize any ad breaks that start on or before the live edge\.  

![\[In the center of the image is a live content stream interspersed with three ad breaks, above a timeline from start to 1+ hours. Each ad break says "fill ad break." Bisecting the third ad break is a vertical dotted green line that represents the live edge.\]](http://docs.aws.amazon.com/mediatailor/latest/ug/images/ad_supp_value_sync_live_edge.png)

**Example 3: Value behind live edge**  
When the **avail suppression value** is behind the live edge, MediaTailor won't personalize any ad breaks on or before that time\. In this example, MediaTailor *will* personalize ad breaks that start within 45 minutes behind the live edge, but *won't* personalize ad breaks that start on or after 45 minutes behind the live edge\.   

![\[In the center of the image is a live content stream interspersed with three ad breaks, above a timeline from start to 1+ hours. There is a vertical green dotted line labeled as the live edge that bisects the third ad break along the timeline. There is a vertical purple dotted line that bisects the first ad break in the content stream labeled avail suppression value. This ad break is dark gray and is labeled ad break personalization.\]](http://docs.aws.amazon.com/mediatailor/latest/ug/images/ad_supp_value_offset_live_edge.png)

## Configuring ad suppression parameters – playback session request<a name="configuring-ad-suppression-parameters-playback-session-request"></a>

You can configure ad suppression settings via parameters in your *initial* server\-side or client\-side playback session request to MediaTailor\. If you've already configured ad suppression settings via the MediaTailor Console or AWS Elemental MediaTailor API, these parameters override those settings\. Both the avail suppression mode and avail suppression value are required for ad suppression to work\. These parameters can't be configured from different sources\. For example, you can't configure one parameter via the Console and another via a query parameter\.

MediaTailor supports the following ad suppression parameters\.


| Name | Description | 
| --- | --- | 
| availSuppressionMode |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/mediatailor/latest/ug/ad-suppression.html)  | 
| availSuppressionValue |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/mediatailor/latest/ug/ad-suppression.html)  | 

### Server\-side configuration<a name="server-side-query"></a>

The base query parameter is `aws.availSuppression`, which is followed by optional parameter name and value pairs\. To construct the query, append `aws.availSuppression=` to the end of the playback session request to MediaTailor, followed by parameter names and values\. For more information about how to construct a server\-side playback session request, see [Server\-side tracking](ad-reporting-server-side.md)\.

**Example**: HLS

```
GET <mediatailorURL>/v1/master/<hashed-account-id>/<origin-id>/index.m3u8?aws.availSuppressionMode=BEHIND_LIVE_EDGE&aws.availSuppressionValue=00%3A00%3A21
```

Server\-side query syntax is listed in the following table\.


| Query String Component | Description | 
| --- | --- | 
| ? | A restricted character that marks the beginning of a query\. | 
| aws\. | The base query, which is followed by parameters constructed of name and value pairs\. For a list of all of the available parameters, see [Configuring ad suppression parameters – playback session request](#configuring-ad-suppression-parameters-playback-session-request)\.  | 
| = | Used to associate the parameter name with a value\. For example, aws\.availSuppressionMode=BEHIND\_LIVE\_EDGE\. | 
| & | Concatenates query parameters\. For example, aws\.availSuppressionMode=BEHIND\_LIVE\_EDGE&aws\.aws\.availSuppressionValue=00:30:00\. | 

### Client\-side configuration<a name="client-side-configuration"></a>

Include `availSuppression` parameters in your client's POST request to MediaTailor\. For more information about how to construct a client\-side playback session request, see [Client\-side tracking](ad-reporting-client-side.md)\.

**Example**: HLS

```
POST master.m3u8
    {
       "availSuppression": {
          "mode": "BEHIND_LIVE_EDGE"
          "value": "00:00:21"
      }
    }
```