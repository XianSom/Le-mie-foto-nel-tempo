SUPSI 2025  
Corso d’interaction design, CV429.01  
Docenti: A. Gysin, G. Profeta  

Elaborato 1: Me, myself and AI  

# Le mie foto nel tempo
Autore: Cristian Sommaiuolo  
[Le mie foto nel tempo](https://github.com/XianSom/Le-mie-foto-nel-tempo.git)


## Introduzione e tema
Nel tempo, accumuliamo immagini come tracce della nostra esperienza quotidiana: momenti fugaci, gesti abituali, eventi speciali. Ma cosa succede quando osserviamo questo archivio non come una raccolta disordinata, ma come un flusso strutturato nel tempo?
"Le mie foto nel tempo" nasce da questa domanda. L'obiettivo non è solo visualizzare le fotografie, ma leggere il tempo attraverso di esse: scoprire abitudini nascoste, picchi emotivi, ricorrenze personali, oppure semplicemente interrogarsi sul nostro modo di osservare il mondo.
Attraverso il data visualization, questo progetto trasforma un archivio fotografico in una forma leggibile, quasi narrativa. Le immagini non sono più isolate: diventano parte di un ritmo, di una cadenza quotidiana o stagionale. Visualizzare che tipo e quante foto scattiamo a certe ore o in certi mesi ci permette di vedere noi stessi da un’altra prospettiva, di interpretare la memoria visiva come una mappa temporale.
In un’epoca in cui produciamo enormi quantità di dati personali, questo lavoro vuole essere una riflessione su come il design dell'informazione possa aiutarci a dare senso al nostro vissuto, attraverso strumenti semplici ma potenti. 


## Design dell’interfaccia e modalità di interazione

La ciclicità del tempo è un'idea che vede il tempo non come una linea retta che procede in un'unica direzione, ma come un ciclo che si ripete continuamente. 
Questa concezione è presente in molte culture e tradizioni antiche, dove gli eventi vengono visti come parte di un ciclo eterno, simile ai cicli naturali come le stagioni o i cicli di vita, morte e rinascita
Per la realizzazione dell'interfaccia ho voluto riprendere la forma del cerchio che rirpende quella di un orologio e il concetto di cicli temporali. 
Per accere all'archivio è stato necessario inserire una prima interazione con un click sul testo "enter" per far approvare dall'utente la riproduzione dei file audio che altrimenti sarebbe bloccata dal browser. 

[<img src="immagini/1.png" width="900" alt="enter">]()

La forma del cerchio viene utilizzata a partire dal primo elemento grafico che funge da barra di caricamento, ma che invece di essere lineare è una circonferenza  bianca che ruota e si colora di rosso quando il caricamento della pagina è stato completato. 

[<img src="immagini/cerchio.png" width="900" alt="loading">]()

Al caricamento della pagina l'interfaccia mostra un istogramma circolare con 24 barre, una per ogni ora della giornata o, alternativamente, tramite un pulsante è possibile cambiare la rappresentazione della distribuzione con 12 barre per i mesi dell’anno. 
L'altezza di ogni barra indica il numero di foto scattate in quella specifica ora o mese selezionato.

[<img src="immagini/ore.png" width="900" alt="Grafico_Ore">]()

[<img src="immagini/mesi.png" width="900" alt="Grafico_Mesi">]()

Cliccando su una specifica barra rossa si apre un pannello laterale dove è possibile visualizzare le foto associate a quell’intervallo temporale. 
In questo modo si ha anche la possibilità di avere una visione d'insieme delle foto per estrapolare pattern visivi o eventuali caratteristiche distintive. 
L'intero istogramma è posizionato in uno spazio tridimensionale e può essere ruotato per poter ottimizzare l'esplorazione dei dati. 
Tramite il click sul pulsante "info" è possibile accedere ad una descrizione del progetto che viene visualizzata in un pannello laterale che si apre da sinistra. 

Lo stile grafico vuole essere il meno invasivo possibile, riprendendo quello di un metodo di analisi scientifico per mantenere una certa oggettività nella fruizione dei dati. 
 



## Tecnologia usata
Per la creazione del sito ho utilizzato diverse AI, in particolare DeepSeek e Copilt all'interno di Visual Code.

```JavaScript
       
const hourlyCounts = [34, 2, 3, 0, 2, 18, 32, 43, 94, 122, 248, 171, 267, 184, 191, 345, 211, 270, 221, 206, 128 , 62, 34, 69];
const monthlyCounts = [120, 210, 180, 250, 300, 270, 220, 190, 160, 140, 130, 126]; 
const monthLabels = ['Gen', 'Feb', 'Mar', 'Apr', 'Mag', 'Giu', 'Lug', 'Ago', 'Set', 'Ott', 'Nov', 'Dic'];

let currentMode = 'hour'; // 'hour' o 'month'

function getChartData(mode) {
  if (mode === 'hour') {
    const N = hourlyCounts.length;
    const theta = Array.from({length: N}, (_, i) => -2 * Math.PI * i / N);
    const radius = 1.5;
    const barTraces = [];
    for (let i = 0; i < N; i++) {
      barTraces.push({
        x: [radius * Math.cos(theta[i]), radius * Math.cos(theta[i])],
        y: [radius * Math.sin(theta[i]), radius * Math.sin(theta[i])],
        z: [0, hourlyCounts[i]],
        mode: 'lines',
        type: 'scatter3d',
        line: { width: 8, color: 'rgba(150,0,0,0.1)' },
        showlegend: false
      });
    }
    const hourLabelsTrace = {
      x: theta.map(t => radius * Math.cos(t)),
      y: theta.map(t => radius * Math.sin(t)),
      z: Array(N).fill(0),
      mode: 'text',
      type: 'scatter3d',
      text: Array.from({length: N}, (_, i) => i.toString()),
      textfont: { color: 'white', size: 16 },
      showlegend: false,
      hoverinfo: 'skip'
    };
    const circlePoints = 100;
    const circleTheta = Array.from({length: circlePoints + 1}, (_, i) => 2 * Math.PI * i / circlePoints);
    const circleX = circleTheta.map(t => radius * Math.cos(t));
    const circleY = circleTheta.map(t => radius * Math.sin(t));
    const circleZ = Array(circlePoints + 1).fill(0);
    const circleTrace = {
      x: circleX,
      y: circleY,
      z: circleZ,
      mode: 'lines',
      type: 'scatter3d',
      line: { width: 4, color: 'white' },
      showlegend: false,
      hoverinfo: 'skip'
    };
    return {traces: [...barTraces, circleTrace, hourLabelsTrace], N, labels: null};
  } else {
    const N = monthlyCounts.length;
    const theta = Array.from({length: N}, (_, i) => -2 * Math.PI * i / N);
    const radius = 1.5;
    const barTraces = [];
    for (let i = 0; i < N; i++) {
      barTraces.push({
        x: [radius * Math.cos(theta[i]), radius * Math.cos(theta[i])],
        y: [radius * Math.sin(theta[i]), radius * Math.sin(theta[i])],
        z: [0, monthlyCounts[i]],
        mode: 'lines',
        type: 'scatter3d',
        line: { width: 14, color: 'rgba(150,0,0,0.1)' },
        showlegend: false
      });
    }
    const monthLabelsTrace = {
      x: theta.map(t => radius * Math.cos(t)),
      y: theta.map(t => radius * Math.sin(t)),
      z: Array(N).fill(0),
      mode: 'text',
      type: 'scatter3d',
      text: monthLabels,
      textfont: { color: 'white', size: 18 },
      showlegend: false,
      hoverinfo: 'skip'
    };
    const circlePoints = 100;
    const circleTheta = Array.from({length: circlePoints + 1}, (_, i) => 2 * Math.PI * i / circlePoints);
    const circleX = circleTheta.map(t => radius * Math.cos(t));
    const circleY = circleTheta.map(t => radius * Math.sin(t));
    const circleZ = Array(circlePoints + 1).fill(0);
    const circleTrace = {
      x: circleX,
      y: circleY,
      z: circleZ,
      mode: 'lines',
      type: 'scatter3d',
      line: { width: 4, color: 'white' },
      showlegend: false,
      hoverinfo: 'skip'
    };
    return {traces: [...barTraces, circleTrace, monthLabelsTrace], N, labels: monthLabels};
  }
}
```

## Target e contesto d’uso
Persone interessate al Data Visualization e a modalità di narrazione interattive in cui la tecnologia viene utilizzata per la fruizione di contenuti multimedialli. 
