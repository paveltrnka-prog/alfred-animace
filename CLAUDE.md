# Alfred – animace (Lottie)

Sbírka Lottie animací (`.json`) pro produkt Alfred. Náhled všech animací je v `_preview.html`
(otevřít přes lokální http server, ne přímo jako `file://` – Lottie player to blokuje):

```
python3 -m http.server 8934
open http://localhost:8934/_preview.html
```

## Struktura

- `*.json` (root) – finální, používané soubory (referencované z `_preview.html`).
- `salto/*.json` – finální soubory pro Salto zámky (samostatná podsložka, součást stejného
  preview).
- `_originals/*.json` – surové exporty (zdroj pravdy pro tvary/geometrii). Root/`salto` verze
  vznikají z nich zpracováním (viz níže). Needit se dokud nevíš, že chceš měnit i zdroj.

Ne každý root soubor má odpovídající `_originals` soubor 1:1 (např. `alfred-idcard-mobile.json`,
`success-checkmark*.json` jsou samostatné/starší).

## Známý vzor chyby: nekonzistentní stroke-width

Při zpracování `_originals` → root/`salto` už se opakovaně stalo, že se tloušťka obrysu (stroke
`w.k` u shape typu `st`) u některých tvarů v souboru nafoukla (např. 2 → 5.8/6.445/9.43), zatímco
jiné tvary ve stejném souboru zůstaly správně. Výsledek: nesourodý, "tlustý" outline oproti
zbytku sady ikon (zejména na kartových/mobilních ikonách).

Diagnostika: porovnat pole stroke-width mezi `_originals/X.json` a `X.json` po vrstvách/shapes ve
stejném pořadí – pokud se v rámci jednoho souboru objeví více různých hodnot tam, kde originál má
jednu konzistentní hodnotu, jde o tuto chybu. Oprava: vrátit width z `_originals` (pokud nejde o
záměrné jednotné zmenšení celé ikony, jako u `key-issuing.json`, kde je celá ikona proporčně
zmenšená a to je v pořádku).

`alfred-idcard-mobile.json` je rastrová (bitmapová) animace, ne vektorová – "tlustý outline" u ní
je zapečený v pixelech obrázku (base64 ve `assets`), nejde opravit úpravou JSONu.

## Aktuální stav rozdělané práce

Viz [`STATE.md`](./STATE.md) – průběžný log oprav (hotovo / čeká), aby se při přerušení práce
nic neztratilo.
