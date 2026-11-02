# Fallback and Reliability

While using Nosto Search Templates, it's possible that Nosto servers will become temporarily unavailable, the user's internet connection temporarily fails or some other issue will disrupt the user experience. To minimize issues and downtime, this article outlines the common strategies for handling outages automatically.

> **Note:**\
> The script examples below assume you have the Nosto stub set up above them.

## Preparing for the fallback

The core idea of the Nosto fallback is to restore the native content that was rendered by the platform. By default, Search Templates Starter restores the content that was on the page before the Nosto content was injected. As such, we recommend you leave the native content rendered even on the page where Nosto content is expected. For example, showing native search results on the search page, and the category listing on the category page.

You may notice that leaving the native content rendered causes it to be briefly visible while the page is loading, before Nosto script hides it. To work around it, you may use a CSS class that hides the content by default.

```html
<div id="nosto-content" class="hidden">
  <!-- Native content here -->
</div>

<!-- Shop styles -->
<style>
  .hidden {
    display: none;
  }
</style>
```

When you'd like to show the content again - for example, on a fallback trigger - you may just strip away the classname. In most cases it's safe to strip it immediately when Nosto is ready as it's going to get hidden by the Starter.

```html
<script>
  // Runs only after Nosto script is loaded
  nostojs(() => {
    document.getElementById("nosto-content").classList.remove("hidden")
  })
</script>
```

If this causes the content to visibly flash anyway, then more targeted code paths to restore the content would be in `src/entries/injected.tsx`'s `init` function's early return, and in `src/fallback/fallback.ts`'s `restoreNativeContent`:

```js
// src/entries/injected.tsx
async function init() {
  if (isFallbackActive()) {
    document.getElementById("nosto-content").classList.remove("hidden")
    return
  }
  // ...the rest of the function
}

// src/fallback/fallback.ts
export function restoreNativeContent() {
  if (location.href !== initialUrl) {
    location.reload()
    return
  }
  unmountTemplate()
  unstashNativeContent()
  document.getElementById("nosto-content").classList.remove("hidden")
}
```

> **Note:**\
> You may fully customize what happens when a fallback is detected. The default behaviour is restoration, but you may opt for a page refresh, a redirect or any other strategy instead. More details below.

## Case 1: Nosto client script fails to load

If the client script completely fails to load or times out, no Nosto code will be loaded on your page at all, meaning any fallback or handling must be located on the native store side. For example, as part of the Shopify theme.

### Detecting outright failures

When adding the Nosto script to your page, you may specify an `onerror` attribute and react to a failed load:

```html
<script>
  function unhideNativeContent() {
    console.warn("Nosto failed to load")
    document.getElementById("nosto-content").classList.remove("hidden")
  }
</script>
<script
  async
  src="//connect.nosto.com/include/$accountID"
  onerror="unhideNativeContent()"
>
</script>
```

### Detecting timeouts

If the script load request never finishes (or takes a long time to finish), you may want to interrupt it and render the native content early. The cleanest way to achieve that is to set up a maximum wait time, and clear it when Nosto is ready.

```html
<script>
  function unhideNativeContent() {
    console.warn("Nosto failed to load")
    document.getElementById("nosto-content").classList.remove("hidden")
  }

  /**
   * 10 seconds.
   * Some devices are slower than others, so false positive triggers are possible.
   * If your page includes a high number of tracking scripts or third-party integrations, be conservative with the maximum wait time.
   */
  const maximumWait = 10 * 1000
  const timeoutId = setTimeout(unhideNativeContent, maximumWait)

  // Nosto script has loaded -> drop the timeout
  nostojs(() => {
    clearTimeout(timeoutId)
  })
</script>
```

## Case 2: Template loads, search request fails

The Search Templates Starter networking code is built with reliability in mind and includes automatic retries to avoid one-off failures. If the search request keeps failing, the fallback mechanism is triggered automatically. You may review the behaviour in Starter at `src/config.ts`, in a function named `onFallback`. You may also customize the search request failure condition to trigger (or not trigger) the fallback in `fallbackOnSearchError`.

On a fallback trigger, the default behaviour is the following:

- If the failure is in Autocomplete, log an error and exit early
  - The expected behaviour is for the user to send the search request, land on a search results page that triggers the fallback.
- Log a warning message
- Disable Nosto integration for this browser tab for a short duration (default: **10 minutes**, saved in session storage)
  - While disabled, Nosto template will not load at all and keep displaying the native content.
- Restore native content

### Customizing the fallback behaviour

If you'd like to react differently by, for example, redirecting to a different page, you may modify the handler function.

```ts
export function onFallback({ pageType, query }: FallbackDetails) {
  if (pageType === "autocomplete") {
    return
  }
  setFallbackSession(fallbackDuration)
  if (pageType === "search") {
   window.location.href = `/search?q=${encodeURIComponent(query?.query ?? "")}`
  } else {
    restoreNativeContent()
  }
}
```