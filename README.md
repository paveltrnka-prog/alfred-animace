# Alfred – animace (Lottie)

Sbírka Lottie animací (`.json`) používaných v produktu **Alfred**.

## Živý náhled

👉 **https://trnkapavel.github.io/alfred-animace/_preview.html**
👉 **https://paveltrnka-prog.github.io/alfred-animace/_preview.html**

## Struktura repozitáře

| Cesta | Popis |
|---|---|
| `*.json` (root) | Finální, používané soubory – referencované z `_preview.html`. |
| `salto/*.json` | Finální soubory pro Salto zámky (samostatná podsložka, součást stejného náhledu). |
| `_originals/*.json` | Surové exporty – zdroj pravdy pro tvary/geometrii. Root/`salto` verze z nich vznikají zpracováním. Needituje se, dokud není potřeba měnit i zdroj. |
| `_preview.html` | HTML náhled všech animací, otevírá se přes lokální HTTP server. |
| `STATE.md` | Průběžný log rozdělané práce (hotovo / čeká). |

Ne každý root soubor má odpovídající soubor v `_originals` 1:1 (např. `alfred-idcard-mobile.json`, `success-checkmark*.json` jsou samostatné/starší).

## Animace v repozitáři

**Root:**
- `alfred-idcard-mobile.json` – rastrová (bitmapová) animace, ne vektorová
- `check-out.json`
- `credit-card-error.json`
- `credit-card-success.json`
- `credit-card-terminal.json`
- `encoder-card-take-and-place.json`
- `encoder-card-wait-to-beep.json`
- `key-issuing.json`
- `mrz-scan.json`
- `open-door-denied.json`
- `open-door-loader.json`
- `open-door-success.json`
- `success-checkmark.json`
- `success-checkmark-check-in.json`

**Salto zámky (`salto/`):**
- `bluetooth_key_communication.json`
- `bluetooth_key_open_fail.json`
- `bluetooth_key_open_success.json`
- `put_phone_closer_to_lock.json`

## Lokální spuštění náhledu

Lottie player blokuje načítání přes `file://`, proto je potřeba lokální HTTP server:

```bash
python3 -m http.server 8934
open http://localhost:8934/_preview.html
```

## Známý vzor chyby: nekonzistentní stroke-width

Při zpracování `_originals` → root/`salto` se opakovaně stalo, že se tloušťka obrysu (stroke `w.k` u shape typu `st`) u některých tvarů v souboru nafoukla (např. 2 → 5.8/6.445/9.43), zatímco jiné tvary ve stejném souboru zůstaly správně. Výsledek: nesourodý, "tlustý" outline oproti zbytku sady ikon.

**Diagnostika:** porovnat pole stroke-width mezi `_originals/X.json` a `X.json` po vrstvách/shapes ve stejném pořadí – pokud se v rámci jednoho souboru objeví více různých hodnot tam, kde originál má jednu konzistentní hodnotu, jde o tuto chybu.

**Oprava:** vrátit width z `_originals` (pokud nejde o záměrné jednotné zmenšení celé ikony, jako u `key-issuing.json`, kde je celá ikona proporčně zmenšená a to je v pořádku).

`alfred-idcard-mobile.json` je rastrová animace – "tlustý outline" u ní je zapečený v pixelech obrázku (base64 ve `assets`), nejde opravit úpravou JSONu.
