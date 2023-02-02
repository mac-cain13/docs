---
title: Adding Plausible to Shopify (and tracking custom events)
---

import useBaseUrl from '@docusaurus/useBaseUrl';

Here's how to add Plausible Analytics to your Shopify site and set up the tracking of custom events such as button clicks and form submissions.

## How to add Plausible to your Shopify website 

* Login into your Shopify account and click on Sales Channels > Online Store > Themes in the left hand side menu.  

* Click the "meatballs" button next to your current theme and from the drop-down menu choose "Edit code". 

<img alt="Add Plausible to Shopify" src={useBaseUrl('img/add-custom-code-to-shopify.png')} />

* In the "Layout" folder, select your "theme.liquid" file and [paste your Plausible snippet](https://plausible.io/docs/plausible-script) in the "**Head Code**" section. Your Plausible tracking script code will look something like this (your exact code will be shown on the JavaScript snippet page in your Plausible account):

```html
<script defer data-domain="yourdomain.com" src="https://plausible.io/js/script.js"></script>
```

<img alt="Add Plausible Analytics script to Shopify" src={useBaseUrl('img/add-plausible-script-to-shopify.png')} />

* Do click on the "**Save Changes**" to publish your changes. 

Now you can go to your Shopify website and verify that Plausible script has been added and to your Plausible account to see whether the stats are being tracked. See here [how to verify the integration](troubleshoot-integration.md).

## How to track form submissions and buttons on your Shopify site

Here's how you can track form submissions on your Shopify site ( same process buttons, they all have IDs ):

### 1. Change the Plausible snippet on your site

Please change the file name in the `src` attribute of your Plausible snippet from `script.js` to `script.tagged-events.js`. It should look like this:

```html
<script defer data-domain="yourdomain.com" src="https://plausible.io/js/script.tagged-events.js"></script>
```

:::note
If you're using outbound link clicks, file downloads or any of our other script extensions, you can [combine them](script-extensions.md#you-can-combine-extensions-according-to-your-needs) by changing the `src` attribute in the snippet. If you want to track custom events and outbound link clicks simultaneously, change the script name to `script.tagged-events.outbound-links.js`.
:::

### 2. Find the ID attribute of the form element you want to track

Your form element should have an ID attribute assigned by default. You can find this ID by selecting the form element you want to track (do make sure you select your form element and not just the "Submit" button) and clicking on the settings gear.


<img alt="Shopify form ID" src={useBaseUrl('img/shopify-form-id.png')} />

### 3. Trigger custom events with JavaScript on your site

Here's the code you will need to insert in the `<head>` section of the page where the element ID that you want to track is located. You can use the "**Custom Code**" feature to do this similarly to how you've inserted the Plausible snippet into your site.

Make sure to change the `elementId` line in the code below to include the ID attribute of the element you want to track (`ContactForm` in our example). 

Also do change the `classes` line to include the goal name in this format: `plausible-event-name=Goal+Name`. The goal name is completely up to you. It's the name under which the goal conversions will appear in your Plausible dashboard. We've used `Form+Submit` goal name in our example.

:::note
To represent a space character in goal names, you can use a `+` sign. For example: `plausible-event-name=Form+Submit` will display as `Form Submit` in your Plausible dashboard
:::

```html
<script>
    var toTag = [
        {
            elementId: 'email-form',
            classes: 'plausible-event-name=Form+Submit'
        }
    ]

    document.addEventListener('DOMContentLoaded', function (_e) {
        toTag.forEach(function (tagObject) {
            var element = document.getElementById(tagObject.elementId)
            tagObject.classes.split(' ').forEach(function (className) {
                if (element) { element.classList.add(className) }
            })
        })
    })
</script>
```

<img alt="Modify Plausible script webflow" src={useBaseUrl('img/modify-plausible-script-shopify.png')} />

Do click on the "**Save Changes**" button to publish your changes.

### 4. Create a custom event goal in your Plausible account

When you send custom events to Plausible, they won't show up in your dashboard automatically. You'll have to configure the goal for the conversion numbers to show up.

To configure a goal, go to [your website's settings](website-settings.md) in your Plausible account and visit the "**Goals**" section. You should see an empty list with a prompt to add a goal.

<img alt="Add your first Shopify goal" src={useBaseUrl('img/add-goal-webflow.png')} />

Click on the "**+ Add goal**" button to go to the goal creation form. Select `Custom event` as the goal trigger and enter the name of the custom event you are triggering. The name must be an exact match to the one you added to your site for the conversions to show up in your dashboard.

So in our example where we added a goal name `plausible-event-name=Form+Submit` to the Shopify site, the goal to add in the Plausible account is `Form Submit` (plus is replaced by a space).

<img alt="Add your custom event goal" src={useBaseUrl('img/form-submission-custom-event-goal-webflow.png')} />

Next, click on the "**Add goal**" button and you'll be taken back to the Goals page. 

### 5. Your goal should now be ready and tracking

Your goal should now be set up. When you navigate back to your Plausible Analytics dashboard, you should see the number of visitors who triggered the custom event. Goal conversions are listed at the very bottom of the dashboard. The goal will show up in your dashboard as soon as it has been completed at least once.

## Tracking button clicks and other links on your Shopify site

In Shopify, link and button elements don't have a default ID. You'll need to assign an ID by selecting the element and clicking on the settings gear. In the **"Designer View"**, you'll be able to set the ID. 

<img alt="Add an ID to link and button elements in Shopify" src={useBaseUrl('img/add-id-to-link-and-button-webflow.png')} />

When you've set the ID to the button or link element that you want to track, you can follow the instructions for how to track form submissions to set up the custom event. The process is the same.

## Triggering multiple custom events on the same page

If you want to trigger multiple custom events on the same site, you don't need to add the script for each element that you want to track. Simply add all the elements in the same code. Make sure to **only add** the elements that already exist on your site. For example, if you want to track a form and a button, the code will look like this:

```html
<script>
    var toTag = [
        {
            elementId: 'ContactForm',
            classes: 'plausible-event-name=Form+Submit'
        },
		{
      		elementId: 'button-ID',
            classes: 'plausible-event-name=Button+Click'   
   		}
    ]

    document.addEventListener('DOMContentLoaded', function (_e) {
        toTag.forEach(function (tagObject) {
            var element = document.getElementById(tagObject.elementId)
            tagObject.classes.split(' ').forEach(function (className) {
                if (element) { element.classList.add(className) }
            })
        })
    })
</script>
```

<img alt="track multiple elements in Shopify" src={useBaseUrl('img/track-multiple-elements-shopify.png')} />