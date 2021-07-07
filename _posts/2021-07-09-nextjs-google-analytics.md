---
layout: post
title: "How I added Google Analytics with cookie consent to my Next.js site"
date: 2021-07-06 14:30
tags: nextjs google-analytics cookies
navigation: true
class: post-template
current: post
---

Recently at work I needed to add google analytics to our company website, built with Next.js.

We were participating in a local event and since there would potentially be a lot of eyes on our site we wanted to start measuring where our traffic was coming from. I figured this would be a quick task that would involve adding a couple tags to our site and that would be it. Such tasks with our website are never this simple, however, and I ended up fighting with our code for quite some time.

In the end I got it working and it's not overly complicated - just took me some time to get there!

I started out by adding the google analytics tags to the site alone, without the cookie banner. We later decided that it would be best to also include a cookie banner, even though it may not be necessary where our business is located.

### Background on cookies and privacy

I need to make a disclaimer right here that I am by no means an expert in privacy law. The conclusions I came to in my research may not be entirely correct, but I will share what I found anyway.

Our business is located in Canada, and it is most likely we would be trying to work with other local companies rather than marketing our product abroad.

The important legislation for us to look at is CASL, Canada's anti-spam legislation. You can [read more about CASL here](https://fightspam.gc.ca/eic/site/030.nsf/eng/home).

You need consent to install software on people's devices, but this [does not apply to cookies](https://crtc.gc.ca/eng/internet/install.htm) or software that one can reasonably assume would be installed while using a particular product. So in Canada it looks like asking for consent via a cookie banner is not necessary.

However, if we were to market to Europe directly, we would need to abide by the [GDPR](https://gdpr.eu/cookies/). You must follow several regulations including obtaining consent to use cookies, allowing users to use your service if cookies are declined, and more.

So even though in Canada it doesn't seem to be necessary, we decided to stay on the safe side and add a cookie consent banner.

### My steps

I followed [this tutorial](https://mariestarck.com/add-google-analytics-to-your-next-js-application-in-5-easy-steps/) initially, but had to make some adjustments once we decided to add the cookie banner. Follow this if you only need the basics and don't need a cookie banner, it's really straight-forward.

<strong>Install [react-cookie-consent](https://www.npmjs.com/package/react-cookie-consent)</strong>

This is what I used for the cookie banner. It saves the user's response, allows you to perform functions on accept/decline, and you can add whatever styling you'd like.

<strong>Add your google analytics code to your .env file</strong>

How you do this depends on what version of Next.js you have. More recent versions allow you to put this directly in a .env file and access it with process.env, while older versions require more work. You just need to be able to access this variable in all your environments, whether dev, prod, etc,

<strong>Create a React component for your analytics and add to each page on your site</strong>

Or, add this code into a component that wraps around <em>every</em> page on your site.

```js
import React, {useState, useEffect} from "react";
import Head from "next/head";
import CookieConsent, {getCookieConsentValue} from "react-cookie-consent";

export default (props) => {
  const [analyticsConsent, updateAnalyticsConsent] = useState("denied");

  {
    /* gets the user's response from CookieConsent when you load a new page */
  }
  const cookieConsent = getCookieConsentValue();

  useEffect(() => {
    if (cookieConsent === "true") {
      updateAnalyticsConsent("granted");
    }
    if (cookieConsent === "false") {
      updateAnalyticsConsent("denied");
    }
  }, []);

  return (
    <div>
      {/* Global Site Tag (gtag.js) - Google Analytics */}
      {analyticsConsent === "granted" && (
        <Head>
          <script
            async
            src={`https://www.googletagmanager.com/gtag/js?id=${process.env.NEXT_PUBLIC_GOOGLE_ANALYTICS}`}
          />{% raw %}
          <script
             dangerouslySetInnerHTML={{
              __html: `
             window.dataLayer = window.dataLayer || [];
            function gtag(){dataLayer.push(arguments);}
            gtag('consent', 'default', {
              'analytics_storage': '${analyticsConsent}'
            });
            gtag('js', new Date());
            gtag('config', '${process.env.NEXT_PUBLIC_GOOGLE_ANALYTICS}', {
              page_path: window.location.pathname,
            });
          `
            }}
          />{% endraw %}
        </Head>
      )}

      {/* saves the user's response */}
      <CookieConsent
        enableDeclineButton
        onDecline={() => {
          updateAnalyticsConsent("denied");
        }}
        onAccept={() => {
          updateAnalyticsConsent("granted");
        }}
        buttonText="Accept"
        declineButtonText="Decline"
        ariaAcceptLabel="Accept cookies"
        ariaDeclineLabel="Decline cookies"
      >
        We use cookies to enhance the user experience and analyze site traffic.
        For more information please read our{" "}
        <a href="/privacypolicy">privacy policy.</a>
      </CookieConsent>
    </div>
  );
};
```

There are two scripts you need to add to your head tag. These initialize google analytics and allow you to start collecting data.

The CookieConsent component is used here and saves the user's answer. I added my own functions on button click to update my analyticsConsent variable, which is saved in state with the useState hook.

The analyticsConsent value is used in the conditional wrapped around the script tags. The default value is set to "denied" so script tags only are added to the head when a user clicks "Accept".

I also added in this bit of code to the script:

```js
gtag("consent", "default", {
  "analytics_storage": "${analyticsConsent}",
});
```

I'm not sure if it's even necessary at this point, since I made the scripts conditional on consent being given. I kept it anyway just to be safe. You can read more about this [in the google analytics docs](https://developers.google.com/gtagjs/devguide/consent). My idea was to substitute in "denied" or "granted" where the "analytics consent" variable is currently. This probably would have worked fine but I felt better not including any google scripts until the user opted in.

#### The \_document.js file

I should note here that the tutorial tells you to add your script tags to the \_document.js file. This works well if you don't need to worry about cookie consent as it adds the scripts to the head tag on all pages.

I didn't do this in the end. It worked better for me to create a separate component to handle the analytics scripts and cookie banner.

<strong>Create a folder in the root of your project called lib, with a file called gtag.js inside</strong>

You will need this code:

```js
// log the pageview with their URL
export const pageview = (url) => {
  window.gtag("config", process.env.NEXT_PUBLIC_GOOGLE_ANALYTICS, {
    page_path: url,
  });
};

// log specific events happening.
export const event = ({action, params}) => {
  window.gtag("event", action, params);
};
```

This helps you track page views and events.

<strong>Import gtag.js into your \_app.js file</strong>

I basically copied this code from the tutorial. You use Next.js router to log page views to google analytics whenever a user navigates to a new page.

```js
import {useEffect} from "react";
import {useRouter} from "next/router";
import CookieConsent, {
  Cookies,
  getCookieConsentValue,
} from "react-cookie-consent";
import * as ga from "../lib/gtag";
import Head from "next/head";

function MyApp({Component, pageProps}) {
  const router = useRouter();

  let cookieConsent = getCookieConsentValue(); //this function comes from react-cookie-consent package

  useEffect(() => {
    if (cookieConsent === true) {
      const handleRouteChange = (url) => {
        ga.pageview(url);
      };
      router.events.on("routeChangeComplete", handleRouteChange);

      return () => {
        router.events.off("routeChangeComplete", handleRouteChange);
      };
    }
  }, [router.events]);

  return <Component {...pageProps}></Component>;
}

export default MyApp;
```

The only changes I made were:

- getting the value of CookieConsent using the function getCookieConsentValue()
- only logging page views if CookieConsent is set to true

Logging page views would only happen if you had google analytics enabled on your site anyway. A problem I ran into, however, was if consent was not given or denied yet, this code would still try to run on page navigation. This would cause an error because the google analytics scripts were not loaded yet, so it doesn't know what "ga" is in the code above. This can only be ran when the scripts are present, which means you need cookie consent first.

### Conclusion

You can do a lot more with google analytics, like logging other types of events besides page views, but basic analytics and consent were enough for us.

This was challenging to figure out with Next.js as I wasn't sure how to properly incorporate cookie consent and have it persist on each page. This was confusing for me as Next.js uses server-side rendering - how would I update my html head tags dynamically in the client? The react-cookie-consent package was very helpful here as I was able to grab the consent value from CookieConsent on any page.

There may be a more elegant way of accomplishing this, but in the end I believe I came up with a relatively simple solution.
