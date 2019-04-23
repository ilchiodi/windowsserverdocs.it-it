# <a name="metadata-and-version-identifiers"></a>Identificatori di versione e metadati

Ecco quello che devi sapere su marchi, controllo delle versioni e i metadati per gli articoli nel repository windowsserverdocs-pr. Gli autori di articolo sono responsabili di garantire che i relativi articoli sono conformi a questi standard e requisiti.

## <a name="trademarks"></a>Marchi
Non usare i simboli marchio dopo i riferimenti a prodotti negli articoli nella raccolta di documentazione tecnica. Non sono necessarie perché il sito Web technet.microsoft.com, docs.microsoft.com e altri Microsoft ufficiale pubblicazione canali includono una [marchio](https://www.microsoft.com/trademarks) collegamento piè di pagina a un elenco dei marchi Microsoft. Tale collegamento soddisfa i requisiti legali. Per informazioni generali, vedere alle linee guida [CELA](https://microsoft.sharepoint.com/sites/LCAWeb/Home/Copyrights-Trademarks-and-Patents/Trademarks/Trademark-List-and-Usage)in "i siti Web" e la Guida di stile Microsoft Writing [copyright e marchi](https://worldready.cloudapp.net/Styleguide/Read?id=2700&topicid=26696) pagina in "Pagine Web in Microsoft.com" 

## <a name="versioning"></a>Controllo delle versioni
Diversi tipi di controllo delle versioni si applicano agli articoli di questo repository: 

-  Controllo delle versioni che indica la versione del prodotto applicabili a un articolo viene eseguita due modi:
    - Manualmente in un'unica riga nell'articolo in modo che venga visualizzato come testo in un articolo pubblicato.
    - In base a metadati, descritto di seguito.
-  Controllo delle versioni del contenuto di origine viene gestita tramite cronologia di Github per il file. 

## <a name="metadata-structure-and-format"></a>Formato e la struttura dei metadati

- Inserire i metadati nella parte superiore del file, sopra l'intestazione H1.
- Separare il blocco dal resto del contenuto del file usando solo tre trattini nelle righe e il cognome del blocco. Non inserire qualsiasi elemento di testo su tali righe.
- Inserire ogni coppia nome/valore dei metadati su una riga separata.
- Usare uno dei modelli di sintassi seguenti, a seconda che i metadati richiede i valori predefiniti o accetta i valori personalizzati. 

        ---
        name1: predefined-value
        name2: "my custom value"
        ---

## <a name="metadata-names-and-values"></a>I valori e i nomi dei metadati

Alcuni metadati sono obbligatorio per tutti i file pubblicati come articoli nella raccolta di documentazione tecnica TechNet, mentre gli altri metadati sono consigliata ma non richiesta. In alcuni casi, sono consentiti solo determinati valori per una specifica porzione di metadati. 

|Nome|Value|
|---|---|
|title|Valore personalizzato che corrisponde all'intestazione H1. Determina ciò che vengono visualizzati nella scheda del browser.|
|description|Viene illustrato nei risultati della ricerca e influisce sul motore di ricerca. Includere le parole chiave appropriate ma mantenere lunghezza inferiore a 160 caratteri.|
|Autore|Alias di Github dell'autore principale dell'articolo|
|ms.author|Alias MSFT di autore o il contenuto per gli sviluppatori dell'articolo responsabili per l'area tecnologica descritto nell'articolo.|
|ms.date|Data dell'ultimo aggiornamento del contenuto. Non aggiornare le modifiche solo metadati. Usare il formato mm/gg/aaaa.|
|ms.prod|Identifica la versione di Windows Server per il report, BI basato su un oggetto predefinito [valore](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969).|
|ms. Technology|Identifica l'area tecnologica per i report, BI basati su un oggetto predefinito [valore](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969)|
|ms.asset|GUID che identifica l'articolo in tutte le impostazioni locali per la creazione di report BI. Articoli migrati da versioni precedenti di authoring sistemi già presente. Per articoli creati in Github, usare uno strumento, ad esempio [ https://guidgenerator.com/ ](https://guidgenerator.com/).| 

## <a name="troubleshooting"></a>Risoluzione dei problemi

I metadati visualizzati nella parte superiore di un articolo pubblicato si verifica quando il file di origine contiene errori di formattazione. Alcuni errori comuni sono:

- Manca il trattino triplo le righe e il cognome di blocco o un numero errato di trattini.
- Metadati non seguono la sintassi richiesta: \<name\>:\<singolo spazio\>\<valore >

Verificare che il file per questi e altri errori ovvi, quindi inviare una richiesta pull per pubblicare il file aggiornato. Se si è rimasti bloccati, messaggio di posta elettronica all'alias di revisori delle richieste pull: wssc-pra@microsoft.com

## <a name="see-also"></a>Vedere anche
I metadati utilizzati in questo repository sono basato sui metadati utilizzato nel contenuto & International \(CSI\). Altre informazioni, inclusi i metadati facoltativi, sono disponibile all'indirizzo [ http://aka.ms/skyeye/meta ](http://aka.ms/skyeye/meta).
Per risorse di business intelligence, vedere il team di BI di CSI [wiki](https://microsoft.sharepoint.com/teams/STBCSI/Insights/Selfserve%20BI%20wiki/Self-serve%20BI%20wiki.aspx).