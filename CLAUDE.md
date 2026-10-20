# Cruscotto Amministrativa

Strumento dell'Ufficio di Polizia Amministrativa della U.O. San Lorenzo (Polizia Locale di Napoli). Genera i documenti Word dell'ufficio: note, report, trasmissioni e riscontri.

## Come lavorare su questo repository

- **Pull request e merge diretti.** Quando una modifica è finita e provata:
  1. fai commit e push;
  2. apri la pull request verso `main`;
  3. uniscila subito, senza chiedere conferma all'utente. Usa il metodo *merge*, come per le PR precedenti.

  Prima del merge controlla che la PR non abbia conflitti. Se ci sono conflitti o controlli rossi, fermati e avvisa l'utente.
- **Dopo il merge** la PR è chiusa. Per la modifica successiva riparti da `main` aggiornato (`git fetch origin main` e poi `git checkout -B <ramo> origin/main`). Non aggiungere commit sopra una storia già unita.
- **Lingua.** Scrivi in italiano messaggi di commit, descrizioni delle PR e testi dell'interfaccia.

## Com'è fatto

- **Un solo file.** Tutto sta in `index.html`: interfaccia, stile e JavaScript. È una pagina statica, senza build.
- **Word.** Il file `.docx` viene costruito nel browser con JSZip (caricato da cdnjs) scrivendo direttamente l'OOXML. Le funzioni condivise sono `para`, `run`, `packDocx`, `protoTableN` e `firmaRow`.
- **Le tab.** Ogni tab è un documento a sé:
  - un pannello `<div id="tab-…" class="tabpane">`;
  - un pulsante `tabBtn…`, registrato in `PANES`/`TABS` in fondo allo script;
  - un prefisso di una lettera per id e funzioni. Per esempio `w` è il report O.S. 25: `wState`, `wRender`, `wBuildPreview`, `buildBodyOs25`.
  - Di solito ogni tab ha anteprima HTML (`…BuildPreview`) e generatore Word (`buildBody…`) affiancati.
- **Rubrica e-grammata.** `RUBRICA`, in cima allo script, è condivisa da tutte le tab. Gli uffici aggiunti a mano restano nel `localStorage` del computer.
  - Nell'anteprima e nel Word i destinatari aggiuntivi si stampano con `xDestHtml(tab)` e `xDestXml(tab)`.
  - Una tab nuova va aggiunta a `X_TABS` e `X_REFRESH`.
- **Stile.** Si seguono i modelli già presenti: testata con stemmi, destinatari a destra con «INDIRIZZO DI PROTOCOLLO», barra del protocollo, «O G G E T T O», firma del Responsabile di Reparto con l'estensore a sinistra.

## Verifica prima del merge

- **Nel browser.** Apri `index.html` in Chromium headless con Playwright, che è preinstallato:
  1. compila i campi della tab;
  2. scarica il Word;
  3. controlla che in console non ci siano errori.
- **Il Word.** Convertilo in PDF con `soffice --headless --convert-to pdf` e guardalo pagina per pagina.
