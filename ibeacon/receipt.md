---
layout: ibeacon
---

## Thank you for your order!

<script>

function getCookie(cname)
{
var name = cname + "=";
var ca = document.cookie.split(';');
for(var i=0; i<ca.length; i++) 
  {
  var c = ca[i].trim();
  if (c.indexOf(name)==0) return c.substring(name.length,c.length);
  }
return "";
}

if(decodeURIComponent(getCookie("lastViewedItemForPurchase")).indexOf("RadBeacon Custom ID") >= 0)  { 
  var order_id = window.location.search.substring(window.location.search.indexOf('amznPmtsOrderIds=')).split("&")[0].replace("amznPmtsOrderIds=", "");
  window.location = 'https://docs.google.com/forms/d/1Y4eTiGJZCf41KQYstokpKl1cCOj71QtKy4Vcr5pUPk0/viewform?entry.1377170050='+order_id; 
}


</script>
