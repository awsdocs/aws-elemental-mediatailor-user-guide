# Prefetching ads<a name="prefetching-ads"></a>

With ad prefetching, AWS Elemental MediaTailor proactively fetches ads from the ad decision server \(ADS\) and prepares them for upcoming ad breaks\. Ad prefetching helps to maximize ad fill rates and monetization in live workflows that use SCTE\-35 signaling, where ad request and transcoding timeouts can occur\. Ad prefetching provides more time for programmatic ad trading\. It also reduces ad insertion latency because both MediaTailor's transcoding of new assets and the ADS response run in the background\.

To set up ad prefetching, you create one or more *prefetch schedules* on your playback configuration\. A prefetch schedule tells MediaTailor how and when to retrieve and prepare ads for an upcoming ad break\. Each prefetch schedule defines a single set of ads for MediaTailor to place in a single ad break\. To prefetch ads for multiple ad breaks, you can create multiple prefetch schedules\. When you create a prefetch schedule, you can include criteria that gives you granular control over which ad break and which playback stream MediaTailor places the prefetched ads in\.

To create and manage prefetch schedules, you can use the MediaTailor console or the MediaTailor API\.

**Topics**
+ [How it works](#understanding-prefetching)
+ [Creating prefetch schedules](#creating-prefetch-schedules)
+ [Deleting prefetch schedules](#deleting-prefetch-schedules)

## How it works<a name="understanding-prefetching"></a>

When your client makes a manifest request to MediaTailor, the service evaluates all of the prefetch schedules that are associated with the playback configuration\. If MediaTailor doesn't find a matching prefetch schedule, the service reverts to normal ad insertion and doesn't prefetch ads\.

If MediaTailor finds a matching prefetch schedule, the service evaluates the schedule based on two components, retrieval and consumption\.

**Retrieval**  
This defines the *retrieval window*, which is the time range when MediaTailor prefetches ads from the ADS\. To set up the retrieval window, first determine when the ad break will occur\.  
For advanced use cases, you can optionally add [*dynamic variables*](variables.md) to the prefetch request that MediaTailor sends to the ADS\. This lets you send session, player, and other data to the ADS as part of the request\. If you don't include dynamic variables in the prefetch schedule, MediaTailor uses the dynamic variables, if any, that you configured in your playback configuration's ADS URL\.

**Consumption**  
This defines the *consumption window*, which is the time range when MediaTailor places prefetched ads into the ad break\.  
For this component, you can optionally add as many as five [*avail matching criteria*](#avail-matching-criteria) to a prefetch schedule\. MediaTailor uses these criteria to determine whether the ad break is eligible for placement of the prefetched ads\. For example, you can use the [*`scte.event_id`*](variables.md) dynamic variable if you want the service to place ads in an ad break with a specific SCTE event ID\. MediaTailor places the prefetched ads into an ad break only if the ad break meets the criteria defined by the dynamic variables\.

When your client sends manifest requests to MediaTailor during the retrieval window, MediaTailor proactively sends requests to the ADS to retrieve and prepare the ads for later insertion\. If you set up dynamic variables for retrieval, MediaTailor includes those variables in the requests\.

When MediaTailor encounters an SCTE\-35 ad break marker during the consumption window, the service uses the avail matching criteria, if configured, to determine which ad break to place the ads in\. If avail matching criteria aren't configured, MediaTailor places the prefetched ads in the first ad break within the consumption window\.

### Understanding prefetching costs<a name="billing"></a>

For prefetch ad retrieval, you'll be charged at the standard transcoding rate for the prefetched ads that MediaTailor transcodes\. For prefetch ad consumption, you'll be charged at the standard rate for ad insertion for the prefetched ads that MediaTailor places in ad breaks\. For information about transcoding and ad insertion costs, see [AWS Elemental MediaTailor Pricing](https://aws.amazon.com/mediatailor/pricing/)\.

## Creating prefetch schedules<a name="creating-prefetch-schedules"></a>

The following procedure explains how to create a prefetch schedule by using the MediaTailor console\. For information about creating and managing prefetch schedules programmatically using the MediaTailor API, see [PrefetchSchedules](https://docs.aws.amazon.com/mediatailor/latest/apireference/prefetchschedule-playbackconfigurationname-name.html) in the *AWS Elemental MediaTailor API Reference*\.

**Note**  
If you want to use avail matching criteria in a schedule, make sure that you first configure your playback configuration's ADS URL template with [dynamic variables](variables.md), otherwise the avail matching criteria won't have an effect\. For information about working with dynamic variables, see [Step 3: Configure ADS request URL and query parameters](getting-started-ad-insertion.md#getting-started-configure-request) in the *Getting started with MediaTailor* ad insertion topic\.

**To create a new prefetch schedule using the console**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Configurations**\. Select the playback configuration that you want to create a prefetch schedule for\.

1. On the **Prefetch schedules** tab, choose **Add prefetch schedule**\.

1. Under the **Prefetch schedule details** pane, do the following:
   + For **Name**, enter an identifier for your prefetch schedule, such as **my\-prefetch\-schedule**\.
   + For **Stream ID**, optionally enter a unique ID\. If your origin contains multiple playback streams, you can use this ID to instruct MediaTailor to place ads in a specific stream\. For example, if your origin has a sports stream and a TV show stream, you can use the stream ID to create prefetch schedules to insert ads targeted for the sports stream\. You pass the stream ID value to MediaTailor in your client's session initialization or manifest request\. For more information see the following example\.
     + For *server\-side tracking*, include the `?aws.streamId` query parameter and value in your client's `GET HTTP` request to your MediaTailor endpoint\. For general information about server\-side tracking see [Server\-side tracking](ad-reporting-server-side.md)\. A manifest request to an HLS endpoint that includes a stream ID looks like the following, where `myStreamId` is the name of your stream ID:

       ```
       GET <mediatailorURL>/v1/master/<hashed-account-id>/<origin-id>/<asset-id>?aws.streamId=myStreamId
       ```
     + For *client\-side tracking*, include the `streamId` key and value in your client's `POST HTTP` session initialization request body to the **MediaTailor/v1/session** endpoint\. For general information about client\-side tracking see [Client\-side tracking](ad-reporting-client-side.md)\. A session initialization request that includes a stream ID looks like the following, where `myStreamId` is the name of your stream ID:

       ```
       POST <mediatailorURL>/v1/session/<hashed-account-id>/<origin-id>/<asset-id>
       {
           'streamId': 'myStreamId'
       }
       ```

1. On the **Retrieval** pane, specify the retrieval settings you want to use\. These settings determine when MediaTailor prefetches ads from the ADS\. They also determine which dynamic variables to include in the request to the ADS, if any\.
   + For **Start time**, enter the time when MediaTailor can start prefetch retrievals for this ad break\. MediaTailor will attempt to prefetch ads for manifest requests made by your client on or after this time\. The default value is the current time\. If you don't specify a value, the service starts prefetch retrieval as soon as possible\.
   + For **End time**, enter the time when you want MediaTailor to stop prefetching ads for this ad break\. MediaTailor will attempt to prefetch ads for manifest requests that occur at or before this time\. The retrieval window can overlap with the consumption window\.
   + In the [**Dynamic variables**](variables.md) section, enter as many as 100 dynamic variables\. MediaTailor uses these variables for substitution in prefetch requests that it sends to the ADS\. If you don't enter any dynamic variables, MediaTailor makes a best effort attempt to interpolate the values for the dynamic variables contained in your [ADS](configurations-create.md#configurations-create-main) URL\.
     + Select **Add dynamic variable**\. 
     + For **Key**, enter a dynamic variable key, such as `scte.event_id`\. You can use any dynamic variable that MediaTailor supports\. For information about dynamic variables, see [Using dynamic ad variables in AWS Elemental MediaTailor](variables.md)\.
     + For **Value**, enter a dynamic variable value, such as *my\-event*\.
     + To add another dynamic variable, choose Select **Add dynamic variable**\. 

1. On the **Consumption** pane, specify the settings that you want to use for the consumption window\. These settings determine when MediaTailor places the ads into the ad break\. They also determine any avail matching criteria that you want to use\.
   + For **Start time**, enter the time when you want MediaTailor to begin to place prefetched ads into the ad break\. the default value is the current time\. If you don't specify a time, the service starts prefetch consumption as soon as possible\.
   + For **End time**, enter a time when you want MediaTailor to stop placing the prefetched ads into the ad break\. MediaTailor will attempt to prefetch ads for your client's manifest requests that occur at or before this time\. The end time must be after the start time, and less than one day from now\. The consumption window can overlap with the retrieval window\.
   + In the [**Avail matching criteria**](variables.md) section, select **Add avail criteria** and add as many ad five avail matching criteria to your schedule\. Then, under **Dynamic variable key**, add a dynamic variable key, such as `scte.event_id`\. MediaTailor will place the prefetched ads into the ad break only if it meets the criteria defined by the dynamic variable values that either your client passes to MediaTailor, or that MediaTailor infers from information such as session data\. For information, see the preceding section [](#avail-matching-criteria)\.

1. Select **Add avail criteria**\.

Prefetch schedules automatically expire after the consumption window's end time\. For diagnostic purposes, they remain visible for at least 7 days, after which MediaTailor automatically deletes them\. Alternatively, you can manually delete a prefetch schedule at any time\. For information about how to manually delete a prefetch schedule, see the following [Deleting prefetch schedules](#deleting-prefetch-schedules) section\.

### Determining how often your client should call the CreatePrefetchSchedule API<a name="how-often"></a>

Your client can programmatically call the [CreatePrefetchSchedule](https://docs.aws.amazon.com/mediatailor/latest/apireference/prefetchschedule-playbackconfigurationname-name.html#prefetchschedule-playbackconfigurationname-name-http-methods.html) API once per day to set up retrieval and consumption if you have knowledge of exactly when ad breaks will occur\. Or, your client can call the API many times over the course of the day to define retrieval and consumption\. When choosing an API call frequency, take into consideration MediaTailor's [maximum number of active prefetch schedules](quotas.md#prefetch-schedules-limit), and the likelihood of whether your ad break schedule will change after you create your prefetch schedule\(s\)\. If it's likely that the ad break schedule will change after you've created your prefetch schedule\(s\), you might want to call the API more frequently\.

## Deleting prefetch schedules<a name="deleting-prefetch-schedules"></a>

The following procedure explains how to delete a prefetch schedule by using the MediaTailor console\. For information about how to delete prefetch schedules programmatically using the MediaTailor API, see [DeletePrefetchSchedule](https://docs.aws.amazon.com/mediatailor/latest/apireference/prefetchschedule-playbackconfigurationname-name.html) in the *AWS Elemental MediaTailor API Reference*\.

**Note**  
Deletion doesn't occur in real\-time\. You might experience a delay while MediaTailor deletes the prefetch schedule\(s\), during which time prefetch retrieval and consumption will continue to run in the background\.

**To delete a prefetch schedule using the console**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Configurations**\. Select the playback configuration that contains the prefetch schedule\(s\) that you want to delete\.

1. On the **Prefetch schedules** tab, select the prefetch schedule that you want to delete\. Then, choose **Delete**\.