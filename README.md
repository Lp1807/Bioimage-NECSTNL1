# Bioimage-NECSTNL1

## Prima analisi

Per prima cosa ho analizzato il dataset e mi rendo conto che le immagini hanno dimensione 512x512, ad eccezione del paziente 160, numero di *slice* dei pazienti non è uniforme. Il dataset ha a disposizione solo la ************maschera************ per i primi 209 pazienti che idealmente costituirebbero il quantitativo di dati necessario per training e validation, con i restanti casi dedicati al testing. 

Tuttavia, per i limiti hardware dovuti alla versione gratuita di Colab mi limito ad utilizzare i primi 100 pazienti e un sottoset di slice, scegliendo arbitrariamente per il resampling il minimo presente nel set, ovvero 29 per il caso 61, scalate a 256x256.

## Codice 1

Nel primo codice `binary_segmentation.ipynb`, quello funzionante, eseguo una segmentazione di tipo binaria, eliminando nelle maschere la distinzione fra rene e tumori, e ponendo tutti i valori diversi dal background uguali ad 1. In questo caso il dataset e la rete neurale, implementata seguendo il modello della uNet, si è rivelata sufficiente per segmentare i reni dalle risonanze magnetiche originali.

## Codice 2

Caso contrario invece è quello del secondo codice `multiclass_segmentation.ipynb` dove provo a distinguere tumori e reni; dapprima triplico le immagini CT così da renderle a tre canali e trasformo le maschere tramite la `one_hot_encoding` così da avere tre indici, ciascuno per ogni classe (background, rene e tumore); in questo modo potevo fare segmentazione multiclasse e aggiungere dei pesi specifici, con la DiceLoss, basati sulla percentuale che ciascuna classe occupava nella maschera. In questo caso il dataset si è rivelato insufficiente e sono stati vani i tentativi di usare reti pretrainate, come la ResNet34 sul dataset di ‘imagenet’, sfruttando la libreria `segmentation_models`, per compensare l’assenza di dati. Si sarebbe potuto aumentare il numero di pazienti per il training/il numero di slice per paziente, o anche fare della data augmentation, specchiando l’immagine o ruotandola, ma non ho potuto verificare sempre per i limiti hardware citati precedentemente.

Mi rendo conto che si poteva risolvere il problema della RAM limitata implementano a mano un iteratore, dato che quello di libreria non supporta il formato `.nii`, seguendo la seguente guida [https://medium.com/analytics-vidhya/write-your-own-custom-data-generator-for-tensorflow-keras-1252b64e41c3](https://medium.com/analytics-vidhya/write-your-own-custom-data-generator-for-tensorflow-keras-1252b64e41c3), così da caricare volta per volta le immagini, e sfruttare l’ImageDataGenerator di Keras eppure, nonostante i miei tentativi, le mie conoscenze pregresse non hanno permesso la sua realizzazione. Una soluzione un po’ più grezza poteva essere quella di convertire gli array numpy in una serie di immagini .jpeg e sfruttare il generatore di libreria ma non ci ho neanche provato sembrandomi una soluzione poco intelligente.

### Riferimenti
[https://www.kaggle.com/code/shakshyathedetector/image-segmentation-using-u-net](https://www.kaggle.com/code/shakshyathedetector/image-segmentation-using-u-net)
https://github.com/qubvel/segmentation_models/blob/master/examples/multiclass%20segmentation%20(camvid).ipynb
https://github.com/neheller/kits19
