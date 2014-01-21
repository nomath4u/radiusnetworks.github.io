---
layout: ibeacon
---

## Thank you for your order!

<script>
if (window.localStorage.getItem("lastViewedItemForPurchase").indexOf("RadBeacon Custom ID") >= 0)  { 
  var order_id = window.location.search.substring(window.location.search.indexOf('amznPmtsOrderIds=')).split("&")[0].replace("amznPmtsOrderIds=", "");
  window.location = 'https://docs.google.com/forms/d/1Y4eTiGJZCf41KQYstokpKl1cCOj71QtKy4Vcr5pUPk0/viewform?entry.1377170050='+order_id; 
}

</script>
