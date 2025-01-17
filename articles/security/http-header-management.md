<!-- Filename: J4.x:Http_Header_Management / Display title: HTTP-headers -->

## Introductie

Joomla 4 introduceerde een HTTP Header-systeem dat is ontworpen om site-eigenaren te helpen bij het configureren van HTTP-beveiligingsheaders. Het wordt geïmplementeerd met behulp van een *Systeem - HTTP Headers*-plugin. Er is een uitgebreide [Gebruiker](jdocmanual?article=user/security/http-headers) handleiding over dit onderwerp. Deze handleiding is korter en behandelt enkele punten die relevant zijn voor ontwikkelaars.

## Het Systeem - HTTP Headers Plugin

### Plugin tabblad

Navigeer naar **Systeem → Plugins → Systeem - HTTP Headers** om het configuratieformulier van de plugin te openen.

![System http headers plugin form](../../../en/images/security/security-http-headers-plugin.png)

- **X-Frame Opties** Dit is standaard ingeschakeld, maar de [documentatie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) zegt dat het verouderd is en dat een *frame-ancestors* beleid gebruikt moet worden.
- **Referrer-Policy** De standaard is *strict-origin-when-cross-origin*.
- **Cross-Origin-Opener-Policy** De standaardwaarde in Joomla is `same-origin`.
- **Forceer HTTP Headers** Er zijn standaard geen geforceerde headers ingesteld. Hier kan een alternatief voor *X-Frame Opties* worden gespecificeerd. De waarde van `Content-Security-Policy` kan een van de volgende zijn:
    - `'none'`
    - `'self' https://www.example.org;`
    - `'self' https://example.org https://example.com https://store.example.com;`

Met het subformulier **Forceer HTTP-headers** kunt u ook de volgende headers afdwingen:

- [Content-Security-Policy](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Content-Security-Policy-Report-Only](https://scotthelme.co.uk/content-security-policy-an-introduction/#testingapolicy)
- [Cross-Origin-Opener-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Opener-Policy)
- [Expect-CT](https://scotthelme.co.uk/a-new-security-header-expect-ct/)
- [Feature-Policy & Permissions-Policy](https://scotthelme.co.uk/a-new-security-header-feature-policy/)
- [NEL (Netwerkresponslogging)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/NEL)
- [Referrer-Policy](https://scotthelme.co.uk/a-new-security-header-referrer-policy/)
- [Report-to](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
- [Strict-Transport-Security](https://scotthelme.co.uk/hsts-the-missing-link-in-tls/)
- [X-Frame-Options](https://scotthelme.co.uk/hardening-your-http-response-headers/#x-frame-options)

### Strict-Transport-Security (HSTS)-tabblad

![strict transport security settings](../../../en/images/security/security-http-headers-hsts.png)

Gebruik de knop *Inline Help Wisselen* voor informatie over elke parameter. Geïllustreerde referentie:

[Formulier om HSTS preload-status en geschiktheid te controleren of in te stellen](https://hstspreload.org/)

### Content-Security-Policy (CSP) tabblad

![Content security policy options](../../../en/images/security/security-http-headers-csp.png)

Zodra ingeschakeld, kun je de client instellen waar je de geconfigureerde CSP wilt afdwingen, zodat je `site`, `beheerder` of `beide` kunt instellen. Een CSP moet zowel op de frontend als backend worden toegepast. Geïllustreerde referenties:

- [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
- [Nonce](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce)
- [Script en Stijl Hashes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src)
- [Beleidsrichtlijn](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/child-src) src beschrijvingen zijn beschikbaar in het menu van deze pagina.

De laatste optie genaamd *Richtlijn toevoegen* stelt je in staat om de allowlist per richtlijn aan te passen zoals je ze nodig hebt. Bijvoorbeeld, voor de *script-src* richtlijn moet het veld *Waarde* de oorsprongen bevatten waarvan je scripts wilt toestaan te laden.

## Notities

Wanneer je enkele HTTP-beveiligingsheaders rechtstreeks op de server hebt geconfigureerd, kunnen er dubbele vermeldingen zijn.

Controleer de uitvoer van je HTTP-headers in de browserconsole. In Google Chrome of Firefox: Inspecteren → Netwerk → de uitvoer onder Headers. Je kunt dan de headers uitschakelen die dubbele vermeldingen veroorzaken. Controleer ook de console van je browser op mogelijke fouten.

## Extensieontwikkelaars

Het grote voordeel van een Content Security Policy treedt op wanneer de Header alle inline JavaScript en inline CSS blokkeert, waardoor JavaScript-eventhandlers worden beïnvloed via HTML-attributen. Met ingeschakelde browserbescherming worden inline JavaScript en inline CSS ook geblokkeerd voor extensies. Die bescherming is niet standaard ingeschakeld, maar kan door gebruikers worden ingeschakeld.

Waar inline JavaScript en CSS vereist zijn, worden nonce- en hash-ondersteuning opgenomen in de Document-API's. Wanneer ze worden gebruikt, zorgt de kern ervoor dat ze op de witte lijst staan, maar zal nog steeds kwaadaardige code blokkeren.

### Belangrijke opmerkingen voor extensieontwikkelaars

Vanaf Joomla 4.0 het Content Security Policy:

- wordt meegeleverd met de kern.
- is standaard uitgeschakeld.
- kan worden ingeschakeld door sitebeheerders.
- het wordt sterk aanbevolen dat de frontend en backend van je extensie werken met de Content Security Policy ingeschakeld.

Met een strikt Content Security Policy ingeschakeld, zullen de volgende functies worden geblokkeerd:

- De uitvoering van JavaScript via de HTML-evenshandlers (onXXX handlers zoals onClick en dergelijke).
- De uitvoering van in-door JavaScript die niet via de Document API aan de pagina is doorgegeven.
- De uitvoering van JavaScript-code geïnjecteerd in DOM-API's zoals eval().
- Het gebruik van inline in-page CSS die niet via de Document API aan de pagina is doorgegeven.
- Het gebruik van inline CSS met behulp van het HTML style-attribuut.

Om uw extensies te laten werken, zelfs met een strikte Content Security Policy ingeschakeld, is de eenvoudigste manier om de Document API te gebruiken om uw inline JavaScript en CSS toe te passen. Zie de voorbeelden hieronder.

### JavaScript toevoegen met de Joomla API

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add JavaScript from URL
    $wa->registerAndUseScript('com_example.sample', 'https://example.org/sample.js', [], ['defer' => true]);

    // Add inline JavaScript
    $wa->addInlineScript('
        document.addEventListener("DOMContentLoaded", function(event) {
            alert("An inline JavaScript Declaration");
        });
    ');
```

### CSS toevoegen met behulp van de Joomla API

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add Style from URL
    $wa->registerAndUseStyle('com_example.sample', 'https://example.org/sample.css');

    // Add inline Style
    $wa->addInlineStyle('
        body {
            background: #00ff00;
            color: rgb(0,0,255);
        }
    ');
```

## Aanvullende bronnen

- [CSP Cheat Sheet](https://scotthelme.co.uk/csp-cheat-sheet/)
- [Content Security Policy - Een Inleiding](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Security Headers](https://securityheaders.com/) is een onderdeel van Probely en werd oorspronkelijk gemaakt door Scott Helme! Het is een gratis en gebruiksvriendelijke tool ontworpen om je te helpen moderne beveiligingsfuncties die beschikbaar zijn voor je website beter uit te rollen en te begrijpen.
- [CSP Evaluator](https://csp-evaluator.withgoogle.com/)
- [Web Fundamentals Content Security Policy](https://developers.google.com/web/fundamentals/security/csp)
- [Google's CSP Documentatie](https://csp.withgoogle.com/docs/index.html)
- [CSP Is Dead, Long Live CSP!](https://research.google/pubs/pub45542/) Over de Onveiligheid van Whitelists en de Toekomst van Content Security Policy
- [web.dev zoekopdracht voor Beveiliging](https://web.dev/s/results?q=security#gsc.tab=0&gsc.q=security&gsc.sort=)

*Vertaald door openai.com*

