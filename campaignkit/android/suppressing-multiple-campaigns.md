---
layout: campaignkit
---

# Suppressing Multiple Campaign Presentations

## Multiple Presentations of the Same Campaign

Each campaign in CampaignKit may be configured on the server with a Campaign Recurrence setting, which allows you to choose whether the campaign is "one time only" or recurs after
a set time interval.  These settings control when a single mobile device will receive a subsequent callback to `didFindCampaign(Campaign campaign)`
when a user enters the place of the campaign more than once.  If this setting is set to "one time only", then the callback will never be issued for that campaign a second time.  If the setting is set to 1 day, a callback will be suppressed for 24 hours after the first one is received.  This suppression is automaticly handled by the SDK.

## Multiple Presentations from Different Campaigns

The automatic suppression above applies only to a single campaign.  If you wish to keep users from being alerted by different campaigns in a short time period, this may be handled by adding custom code in your class that implements the `CKManagerDelegate` protocol.  In the example below, any triggered campaign will be suppressed if any other campaign has been seen in the past hour.

```java

Date lastCampaignDeliveryTime;
...

@Override
public void didFindCampaign(Campaign campaign) {
    
    Date currentTime = new Date();

    if (lastCampaignDeliveryTime != null) {
        if (currentTime.getTime() < (lastCampaignDeliveryTime.getTime() + 3600000l)) {
            Log.d(TAG,"Ignoring campaign because another one has been presented to the user in the past hour.");
            return;
        }
    }
    
    lastCampaignDeliveryTime.setTime(currentTime.getTime());
    
    new CampaignNotificationBuilder(_mainActivity, campaign)
    .setSmallIcon(R.drawable.ic_launcher)
    .setOnClickActivity(DetailActivity.class)
    .show();
        
}
```

