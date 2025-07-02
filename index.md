---
layout: default
title: Tracking Demo
---

<!-- OneTrust Cookies Consent Notice start -->
<script src="https://cdn.cookielaw.org/scripttemplates/otSDKStub.js" type="text/javascript" charset="UTF-8" data-domain-script="0197c80f-6577-7c8f-ac82-f54c2230733f-test"></script>
<script type="text/javascript">
  function OptanonWrapper() { }
</script>
<!-- OneTrust Cookies Consent Notice end -->

<h1>Tracking Simulation Page</h1>
<button id="buy">Buy Now</button>

<script>
  const trackingData = {
    timestamp: new Date().toISOString(),
    page: window.location.pathname,
    referrer: document.referrer,
    userAgent: navigator.userAgent,
    screenWidth: screen.width,
    screenHeight: screen.height,
    clicks: [],
    scrollDepth: 0,
    location: null
  };

  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      (position) => {
        trackingData.location = {
          latitude: position.coords.latitude,
          longitude: position.coords.longitude,
          accuracy: position.coords.accuracy
        };
        console.log("Geolocation obtained:", trackingData.location);
      },
      (error) => {
        console.warn("Geolocation error:", error.message);
      }
    );
  }

  document.getElementById('buy').addEventListener('click', () => {
    trackingData.clicks.push({
      element: 'Buy Button',
      time: new Date().toISOString()
    });
    console.log("Button Click Tracked:", trackingData);
    document.cookie = `clickEvent=BuyButton_${Date.now()}; path=/`;
  });

  window.addEventListener('scroll', () => {
    const scrolled = Math.round(window.scrollY / document.body.scrollHeight * 100);
    trackingData.scrollDepth = scrolled;
  });

  window.addEventListener('beforeunload', () => {
    document.cookie = `lastVisit=${encodeURIComponent(JSON.stringify(trackingData))}; path=/`;
  });

  console.log("Initial Tracking Data:", trackingData);
</script>
