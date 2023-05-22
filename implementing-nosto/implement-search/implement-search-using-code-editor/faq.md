# FAQ

## Share search templates code between multiple accounts

The functionality enables the sharing of a single instance of search templates across multiple dashboard accounts through linking. This feature proves particularly advantageous in scenarios where there are multiple websites with identical or similar characteristics that necessitate the creation of multiple dashboard accounts (such as supporting multiple languages or environments).

{% hint style="info" %}
To link multiple accounts contact support!
{% endhint %}

#### How to implement some differences between websites?

Even if multiple websites share the same search templates code, we can easily have different logic based on website host or language (or other criteria).

```javascript
function detectWebsite() {
    if (location.hostname === 'de.website.com') {
        return 'de'
    } else if (document.documentElement.lang === 'de') {
        return 'de'
    }
    return 'en'
}

const helloText = {
    de: 'Hallo',
    en: 'Hello'
}[detectWebsite()]
```
