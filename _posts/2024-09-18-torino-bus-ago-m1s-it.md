---
layout: post
title: "MetroBus sostitutivo a Torino: l'efficacia ad agosto (Italian)"
date: 2024-09-18 19:00:00
description: 
img: /assets/img/12.jpg
---
Si è molto discusso intorno ad agosto della chiusura per il secondo anno consecutivo della linea della metropolitana, per motivi di aggiornamento tecnologico. Non a caso: è probabilmente l'unica linea del trasporto GTT affidabile, con (relativamente pochi) guasti e disservizi. La linea di Metropolitana ha sicuramente migliorato molto la vita di chi non usa la macchina né la bici, e la sua chiusura totale per mese ha sicuramente portato dei guai. Per sopperire a questa mancanza, GTT ha attivato per tutto il mese di Agosto la linea di superficie M1S, che, come si intuisce dal nome, percorre tutte le fermate della metro, ma in superficie. Di solito è attiva solo la notte (dopo le 21:00 / l'ora in cui chiude la metro nei giorni infrasettimanali ora), e impiega dei bus (tipicamente) autosnodati.

Naturalmente, una linea di bus anche molto potenziata non avrebbe mai potuto sostituire una metropolitana, né per capienza né per frequenza, perciò si sono alzate molte proteste, e voci sia critiche che lusinghiere nei confronti del servizio. 

Dopo aver scaricato le posizioni in tempo reale di tutti i bus e tram ad agosto, ho pensato di fare una piccola analisi sulla effettiva efficacia del MetroBus. Come si vedrà più avanti, non ho avuto dei risultati perfetti, che vanno a favore di una o dell'altra opinione, ma ho potuto trarre alcune conclusioni.
## Qualità dei dati
Il primo aspetto importante è la qualità dei dati. In precedenza mi era capitato di scaricare sia quelli <a href="http://aperto.comune.torino.it/dataset/feed-gtfs-real-time-trasporti-gtt" target="_blank">GTFS Realtime</a> che quelli provenienti da <a href="https://mato.muoversiatorino.it/">Muoversi a Torino</a>, e per quanto gli ultimi fossero aggiornati più spesso (e quindi molto più dettagliati), erano anche pieni di errori, come bus "fantasma" che rimanevano bloccati, o tram che rimanevano con la posizione "attiva" nonostante fossero al deposito. Perciò ho deciso di utilizzare i dati GTFS, sicuramente più corretti: meglio pochi ma buoni.

Naturalmente ci sono stati disservizi anche nel servizio delle posizioni in tempo reale: non sempre, infatti, il server rispondeva con dei dati. Qui sotto viene mostrato il numero di aggiornamenti della posizione, ogni 15 minuti, e si vede chiaramente che ci sono stati dei giorni (più precisamente, il 17 di agosto) in cui il servizio non ha mai funzionato (dov'è bianco nell'immagine). Altre volte, il disservizio è durato da poche decine di minuti a qualche ora. 

Già osservando bene l'immagine sotto, un occhio esperto noterà che alcuni giorni il servizio smette di inviare dati più tardi (e riprende anche più tardi): si tratta del weekend, quando viene attivato il servizio notturno di bus.

 <div class="mt-0 mt-md-0 mx-auto float-end">
        {% include image.html path="assets/img/positions/GTFS_updates_heatmap_horiz_ago2024.png" title="Line sections and trace"  class="float-end img-fluid rounded z-depth-1" %}
    </div>

## Il bus M1S ad agosto
Passiamo ora alla linea sostitutiva della metro. Una prima statistica interessante da vedere è il numero di bus messi in servizio sulla linea, divisi per direzione, ad ogni momento della giornata. Nell'immagine sotto si può vedere esattamente ciò, con il numero di mezzi di nuovo contati a blocchi di 15 minuti (clic per ingrandire).

 <div class="mt-0 mt-md-0 mx-auto float-end" >
        {% include image.html path="assets/img/positions/M1S_bus_count_ago24.png"  class="float-end img-fluid rounded z-depth-1" %}
</div>

Da queste due figure, si vede come il numero di mezzi messi in servizio non sia stato costante. Assumendo che non ci siano stati mezzi che avevano il GPS non funzionante, o che non venivano riportati, si nota come i giorni feriali saltano immediatamente all'occhio per la differenza con il weekend. Si può fare un resoconto delle 4 settimane in questione:
- Nell'ultima settimana, il numero dei mezzi è decisamente aumentato, arrivando a toccare anche i 20 per direzione, mentre nella terza e nella prima settimana si è raggiunto un massimo di 14-15. Nella seconda settimana (di Ferragosto) ce ne sono stati circa una decina. 
- Nella prima e nella terza settimana il numero non è stato costante alla mattina e al pomeriggio (quando se ne trovavano meno), nell'ultima i passaggi si sono intensificati nell'orario 9:30-11:30.

In generale, si vedono molti più mezzi in circolazione nell'ultima settimana di agosto.

## Analisi della velocità
Come fatto in precedenza, ho voluto vedere quale è stata la velocità di percorrenza della linea, e se è cambiata nel tempo. Ho quindi replicato l'analisi fatta molti mesi fa, riscrivendo in pratica il codice da zero. Ho diviso la (lunga) tratta della linea in corrispondenza delle fermate, per evitare di introdurre differenze tra una sezione e l'altra dividendo per sezioni della stessa lunghezza.

Per fare pochi grafici, ho fatto la media sui giorni, separando i giorni feriali da quelli festivi (domenica, sabato e feste come Ferragosto). Qui sotto riporto i grafici della media sui giorni feriali: sull'asse y ci sono le sezioni della linea, sull'asse x l'ora della giornata.     


<div class="mt-0 mt-md-0 mx-auto float-end" style="max-width:500px"     >
        {% include image.html path="assets/img/positions/heatmap_speed_dir_Bengasi_stops.png"  class="float-end img-fluid rounded z-depth-1" %}
</div>

A parte alla tarda notte, o al mattino presto, non sembra esserci grande variazione tra le velocità in ore diverse del giorno. Mentre la velocità molto bassa nella prima sezione (tra le fermate Fermi Sud e Paradiso) potrebbe essere sia reale sia dovuta ad un effetto di "inizio registrazione" (cioè, il bus rimane fermo a lungo al capolinea prima di ripartire), quelle dei tratti Principi D'Acaja - XVIII Dicembre e Porta Susa - Vinzaglio sono spiegabili dalla tortuosità del tracciato, che, diversamente dalla metro, richiede il passaggio a diversi incroci semaforici. 

<div class="mt-0 mt-md-0 mx-auto float-end" style="max-width:500px"     >
        {% include image.html path="assets/img/positions/heatmap_speed_dir_Fermi_stops.png"  class="float-end img-fluid rounded z-depth-1" %}
</div>
In generale, si nota che da Principi d'Acaja a Racconigi e da Rivoli a Pozzo Strada le velocità sono in media più elevate che negli altri tratti, stabilmente sopra i 20 km/h.  Le velocità tendono ad essere più elevate alla sera, ma non di molto (qualche km/h).

## Conclusioni

Naturalmente, una linea di metropolitana non può essere facilmente sostituita da una di superficie, e i dati analizzati lo confermano. Dalle analisi effettuate emergono due risultati:
1. Il numero dei mezzi è variato significativamente tra le settimane: soprattutto in quella di Ferragosto, ma anche nella precedente e nella successiva, ci sono stati meno bus in circolazione. Ciò può essere dovuto sia ad una accurata programmazione che ad una scarsa, dato che nel periodo di Ferragosto molti torinesi sono fuori città. Tuttavia, questa riduzione può aver impattato sul turismo.
2. Per quanto anche in metropolitana si potrebbe osservare una riduzione della velocità di percorrenza tra le stazioni, visto nel tratto Principi D'Acaja - Vinzaglio sono molto ravvicinate, si è vista una riduzione di velocità importante per il M1S nel tratto indicato. La velocità media di percorrenza è variata di più con la linearità del percorso che con l'orario, e ciò può anche essere spiegato dai livelli inferiori di traffico privato del periodo agostano.

Come già anticipato, i dati non danno una conferma definitiva della poca o molta efficacia della linea ad agosto. In generale si può osservare che (come per il resto del trasporto pubblico) la linea M1S sembra realizzata per soddisfare le esigenze di lavoratori e studenti nel raggiungere il proprio ufficio / luogo di lavoro / scuola. 

Ho effettuato tutto questo lavoro di analisi nel mio tempo libero, perciò non sono potuto andare molto a fondo. Se qualche lettore trova inaccuratezze o vorrebbe qualche chiarimento, può scrivermi. In più, sto continuando a raccogliere i dati delle posizioni in tempo reale, e se nel futuro troverò un altro risultato interessante scriverò un post come questo.