# Insert turnstile
```javascript
class ElementHandler {
  element(element, env) {
    element.append(`<script>
    (function() {
      if (sessionStorage.getItem("ts")) return dataLayer.push({ event: "turnStyleOmitted" });;
      console.log("zaraz loaded");
      var tsblock = document.createElement("div");
      tsblock.id = "ts";
      var modal = document.createElement("div");
      modal.style.display = "none";
      modal.appendChild(tsblock);
      document.body.appendChild(modal);
      var onloadTurnstileCallback = function () {
        console.log("ts loaded");
        var tsid = turnstile.render(tsblock, {
          appearance: "interaction-only",
          sitekey: "0x4AAAAAAAYCU50u94XytH6Y",
          action: window.location.hostname.replaceAll('.','_'),
          "refresh-timeout": "none",
          callback: function (token) {
            console.log("Challenge Success token", token);
            modal.style.display = "none";
            turnstile.remove(tsid);
            sessionStorage.setItem("ts", true);
          },
          "before-interactive-callback": function (before) {
             console.log("before", before);
             modal.style =
               "position: fixed;width: 100vw;height: 100vh;left: 0;top: 0;z-index: 9999;background-color: #00000076;display: flex;justify-content: center;align-items: center;backdrop-filter: blur(6px);";
             dataLayer.push({ event: "turnStyleLaunched" });
          },
          "error-callback": function (err) {
            dataLayer.push({ event: "turnStyleError", turnStyleError: err });
            console.log("error", err);
          },
      });
    };
    const script = document.createElement('script');
    script.src = 'https://challenges.cloudflare.com/turnstile/v0/api.js??render=explicit';
    script.type = 'text/javascript';
    script.onload=onloadTurnstileCallback;
    script.defer = true;
    document.head.appendChild(script);
  })();
  </script>`, {html: true});
    console.log("injected");
  }
}

async function handleRequest(req, env) {
  const res = await fetch(req)
    if (!['api'].some(v => res.url.includes(v))) {
    return new HTMLRewriter().on("body", new ElementHandler()).transform(res, env)
  }  
  return res;
}

  
addEventListener("fetch", (event, env) => {
  event.respondWith(
    handleRequest(event.request, env).catch(
      (err) => new Response(err.stack, { status: 500 })
    )
  );
});
```