# Stav rozdělané práce

Datum poslední aktualizace: 2026-07-01

## Hotovo

- `credit-card-success.json`: odstraněna nadbytečná vrstva (bílý vyplněný kruh za zeleným
  kroužkem se checkmarkem), která vytvářela nesmyslný bílý outline. `_originals` verze
  ponechána beze změny (obsahuje stejnou vrstvu – je součástí surového exportu).
- `credit-card-error.json`, `credit-card-success.json`, `credit-card-terminal.json`,
  `mrz-scan.json`: opraven nekonzistentně nafouklý stroke-width (viz `CLAUDE.md` – "Známý vzor
  chyby"). Hodnoty vráceny na width z `_originals`. Ověřeno vizuálně přes `_preview.html`
  (playwright screenshot) – outline teď tenký a konzistentní se zbytkem sady.

- `credit-card-success.json`: bílé pozadí za checkmarkem bylo nutné vrátit zpět (nebyl to bug,
  jen špatná velikost) – v `_originals` je bílý vyplněný kruh o poloměru 32.8, ale zelený
  kroužek má poloměr jen 24.7, takže vznikal halo efekt. Oprava: nová vrstva 4 = kopie geometrie
  zeleného kroužku (vrstva 3) s bílou výplní místo zeleného stroke, vložená hned za ni v pořadí
  vrstev (renderuje se pod kroužkem i checkmarkem). `_originals` beze změny.
- `encoder-card-take-and-place.json`: stejný vzor chyby jako u karet – stroke-width nafouklý
  2 → 9.883 (canvas navíc změněn z 480×253 na 512×512, scale vrstev 100 %→121 %, ale šířka
  obrysu se navíc ručně vynásobila místo aby se škálovala jen přes transform). Tenké linky se
  slily do plných skvrn. Opraveno vrácením width z `_originals` (2). Ověřeno side-by-side
  renderem (`_originals` vs root) – teď identické.
- `key-issuing.json`: ilustrace klíče zmenšena o 30 % (scale i pozice layerů přepočítány kolem
  středu plátna 256,256), proporce a vystředění zachovány.

## Čeká na doladění

- `alfred-idcard-mobile.json` – "tlustý outline" na mobilu/telefonu. Je to rastrová animace
  (base64 obrázky v `assets`), takže oprava přes úpravu JSON tvarů není možná. Čeká na
  rozhodnutí uživatele, jak dál (např. dodat nový export obrázku bez tlustého okraje).

## Poznámka k workflow

Než se pustíme do dalších úprav, ověřit s uživatelem přesně které soubory/prvky ještě chce
opravit, aby nedošlo k přepsání rozdělané práce, o které Claude neví (paměť mezi sessions se
needukuje ze souborů automaticky, pouze z `CLAUDE.md`/`STATE.md`).
