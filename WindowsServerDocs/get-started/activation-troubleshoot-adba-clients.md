---
title: Risoluzione dei problemi di attivazione dei client con attivazione basata su Active Directory
description: Questo articolo illustra un processo di risoluzione di un problema di attivazione di Windows
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: b4e31cfa892019e4f3bbcd3b67dbb42751cc58dd
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963037"
---
# <a name="example-troubleshooting-active-directory-based-activation-adba-clients-that-do-not-activate"></a>Esempio: Risoluzione dei problemi di attivazione dei client con attivazione basata su Active Directory

> [!NOTE]
> Questo articolo è stato originariamente pubblicato sul blog TechNet il 26 marzo 2018.

Ciao a tutti! Il mio nome è Mike Kammer e da poco più di due anni ricopro il ruolo di Platforms Premier Field Engineer presso Microsoft. Di recente ho collaborato alla distribuzione di Windows Server 2016 nell'ambiente di un cliente. L'introduzione di questa nuova piattaforma ha offerto anche l'opportunità di eseguire la migrazione della metodologia di attivazione dall'uso di un server del Servizio di gestione delle chiavi all'[attivazione basata su Active Directory](https://docs.microsoft.com/previous-versions/windows/hh852637(v=win.10)).

Seguendo la procedura corretta per l'esecuzione di tutte le modifiche, abbiamo avviato la migrazione nell'ambiente di test del cliente. Per iniziare la distribuzione abbiamo seguito le istruzioni riportate nell'ottimo post di blog di Charity Shelbourne, [Active Directory-Based Activation vs. Key Management Services](https://techcommunity.microsoft.com/t5/Core-Infrastructure-and-Security/Active-Directory-Based-Activation-vs-Key-Management-Services/ba-p/256016) in cui è illustrato il confronto tra l'attivazione basata su Active Directory e i servizi di gestione delle chiavi. Nei controller di dominio nell'ambiente di test era in esecuzione Windows Server 2012 R2, quindi non è stato necessario preparare la foresta. Abbiamo installato il ruolo in un controller di dominio Windows Server 2012 R2 e abbiamo scelto l'attivazione basata su Active Directory come metodo di attivazione dei contratti multilicenza. Abbiamo installato il codice del Servizio di gestione delle chiavi e assegnato a tale codice il nome "KMS AD Activation ( ** LAB)". Per questa procedura abbiamo seguito passo passo le istruzioni del post di blog.

Abbiamo iniziato creando quattro macchine virtuali, due Windows 2016 Standard e due Windows 2016 Datacenter. Tutto è filato liscio con la soddisfazione generale. Abbiamo creato un server fisico con Windows 2016 Standard e il computer è stato attivato correttamente. Tutto qui. Fine della storia.

In realtà stavo scherzando. Le cose non sono mai così semplici. A dire il vero, la parte relativa all'installazione e alla configurazione è stata molto semplice. Sono tornato in ufficio lunedì e tutte le macchine virtuali che avevo creato la settimana prima non risultavano attivate. Questo non andava bene. Ho controllato il computer fisico ed era tutto a posto. Ho quindi parlato con il cliente per discutere dell'accaduto. Naturalmente la prima domanda è stata "Che cosa è cambiato nel fine settimana?". E, come spesso accade, la risposta è stata "Nulla". Non era cambiato nulla, ma dovevamo capire che cosa stava accadendo.

Sono quindi passato a uno dei server che presentavano problemi, ho aperto un prompt dei comandi e ho controllato l'output dal comando **slmgr /ao-list**. L'opzione **/ao-list** consente di visualizzare tutti gli oggetti di attivazione in Active Directory.

![Immagine che mostra il comando slmgr](./media/032618_1700_Troubleshoo1.png)

![Immagine che mostra i risultati del comando slmgr](./media/032618_1700_Troubleshoo2.png)

I risultati mostrano la presenza di due oggetti di attivazione: uno per Server 2012 R2 e l'oggetto KMS AD Activation (** LAB) appena creato, ovvero la licenza di Windows Server 2016. Questi dati confermano che l'istanza di Active Directory è configurata correttamente per l'attivazione dei client del Servizio di gestione delle chiavi di Windows.

Poiché il comando **slmgr** è un ottimo strumento per gestire i problemi relativi all'attivazione delle licenze, ho continuato con altre opzioni. Ho provato l'opzione **/dlv**, che consente di visualizzare informazioni dettagliate sulla licenza. Anche in questo caso i risultati erano corretti. Era in esecuzione la versione Standard di Windows Server 2016, con un ID di attivazione, un ID di installazione, un URL di convalida e anche un codice Product Key parziale.

![Immagine che mostra i risultati di slmgr /dlv](./media/ActivationTroubleshoot2b.jpg)

Qualcuno ha notato ciò che mi è sfuggito a questo punto? Lo vedremo al termine della mia procedura di risoluzione del problema. Basti sapere che la risposta è in questa schermata.

A questo punto mi viene in mente che, per qualche motivo, il codice possa essere danneggiato e quindi decido di usare l'opzione **/upk** per disinstallare il codice corrente. Anche se questo è un metodo possibile per rimuovere il codice, non è in genere quello migliore per eseguire questa operazione. In caso di riavvio prima dell'acquisizione di un nuovo codice, il server potrebbe trovarsi in uno stato non valido. Ho scoperto che l'opzione **/ipk** (che userò più avanti nella procedura) consente di sovrascrivere il codice esistente e offre una soluzione molto più sicura. I miei errori possono esserti utili.

![Immagine che mostra il comando slmgr /upk e il relativo risultato](./media/032618_1700_Troubleshoo3.png)

Ho eseguito di nuovo l'opzione **/dlv** per visualizzare le informazioni dettagliate sulla licenza. Purtroppo non ho ottenuto informazioni utili, solo un errore relativo al codice Product Key non trovato. Questo perché, ovviamente, ho appena disinstallato il codice.

![Immagine che mostra il comando slmgr /dlv e il relativo risultato](./media/032618_1700_Troubleshoo4.png)

Anche se ho immaginato si trattasse di una soluzione un po' azzardata, ho provato l'opzione **/ato**, che dovrebbe attivare Windows in base ai server del Servizio di gestione delle chiavi noti (oppure Active Directory, a seconda del caso). Anche in questo caso, è stato restituito un errore di prodotto non trovato.

![Immagine che mostra il comando slmgr /ato e il relativo risultato](./media/032618_1700_Troubleshoo5.png)

A questo punto ho pensato che a volte l'arresto e l'avvio di un servizio consentono di risolvere il problema e ho tentato anche questa soluzione. Dovevo arrestare e avviare il servizio SPPSvc, ovvero Microsoft Software Protection Platform Service. Da un prompt dei comandi amministrativo, ho usato i comandi attendibili **net stop** e **net start**. Inizialmente ho notato che il servizio non era in esecuzione e quindi ho pensato che il problema fosse dovuto a questo.

![Immagine che mostra i risultati dei comandi net stop e net start](./media/032618_1700_Troubleshoo6.png)

Ma la soluzione non era nemmeno questa. Dopo l'avvio del servizio e il tentativo di attivazione di Windows, veniva comunque restituito un errore di prodotto non trovato.

Ho quindi esaminato il registro eventi Application in uno dei server con problemi. Ho individuato un errore relativo all'attivazione della licenza, ID evento 8198, con codice 0x8007007B.

![Immagine che mostra i dettagli dell'ID evento 8198](./media/032618_1700_Troubleshoo7.png)

Cercando informazioni su questo codice ho trovato un articolo in cui si spiegava che l'errore era dovuto al nome del file o della directory o alla sintassi dell'etichetta del volume non corretta. I metodi descritti nell'articolo non sembravano adatti alla mia situazione. Eseguendo il comando **nslookup -type=all _vlmcs._tcp**, ho trovato il server del Servizio di gestione delle chiavi esistente (nell'ambiente erano ancora presenti diversi computer Windows 7 e Server 2008 e quindi era necessario mantenerlo), ma anche i cinque controller di dominio. Questo indicava che non si trattava di un problema del DNS e che la soluzione ai miei problemi era da cercare altrove.

![Immagine che mostra i risultati del comando nslookup](./media/032618_1700_Troubleshoo8.png)

A questo punto, sapevo che il DNS funzionava correttamente. Active Directory era configurato in modo corretto come origine di attivazione del Servizio di gestione delle chiavi. Il server fisico era stato attivato correttamente. Poteva forse trattarsi di un problema specifico delle macchine virtuali? Come nota collaterale interessante, a questo punto, il cliente mi ha informato che qualcuno di un altro reparto aveva deciso di creare una decina di macchine virtuali Windows Server 2016. Ho quindi supposto di avere un'altra decina di server non attivati da gestire. E invece no. Gli altri server venivano attivati correttamente.

Sono quindi tornato al mio comando **slmgr** per scoprire come attivare i miei fatidici server. Questa volta ho deciso di usare l'opzione **/ipk**, in modo da installare un codice Product Key. Sono quindi andato su [questo sito](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612867(v=ws.11)) per ottenere i codici appropriati per la versione Standard di Windows Server 2016. Alcuni server sono Datacenter, ma dovevo prima correggere il server Standard.

![Immagine che mostra l'elenco dei codici di configurazione di client del Servizio di gestione delle chiavi](./media/032618_1700_Troubleshoo9.png)

Ho usato l'opzione **/ipk** per installare un codice Product Key, scegliendo il codice per Windows Server 2016 Standard.

![Immagine che mostra il comando slmgr /ipk e i relativi risultati](./media/032618_1700_Troubleshoo10.png)

Da questo punto in poi, ho acquisito solo i risultati dalle mie esperienze di Datacenter, ma erano gli stessi. Per forzare l'attivazione ho usato l'opzione **/ato**. Finalmente è stato restituito il messaggio di conferma dell'attivazione del prodotto.

![Immagine che mostra il comando slmgr /ato e il relativo risultato](./media/032618_1700_Troubleshoo11.png)

Usando nuovamente l'opzione **/dlv**, possiamo osservare che il prodotto è stato attivato tramite Active Directory.

![Immagine che mostra il comando slmgr /dlv e il relativo risultato](./media/032618_1700_Troubleshoo12.png)

Ma allora che cosa non aveva funzionato? Per quale motivo ho dovuto rimuovere il codice installato e aggiungere codici generici per consentire l'attivazione delle macchine virtuali? Per quale motivo le altre macchine venivano attivate senza problemi? Come ho già detto, mi era sfuggita un'informazione fondamentale nelle fasi iniziali dell'analisi del problema. Ero molto confuso e ho quindi contattato Charity, l'autrice del post di blog iniziale, per chiedere un aiuto. Charity ha identificato immediatamente il problema e mi ha aiutato a capire ciò che mi era sfuggito.

Quando ho eseguito l'opzione **/dlv** per la prima volta, la chiave del problema era nella descrizione. Nella descrizione era indicato Windows® Operating System, RETAIL Channel. Avevo notato la descrizione e pensato che RETAIL Channel indicasse la presenza di un codice acquistato valido.

![Immagine che mostra i risultati di slmgr /dlv, con le informazioni sul canale evidenziate](./media/032618_1700_Troubleshoo13.png)

Nell'output dell'opzione **/dlv** per un server attivato correttamente, come descrizione è riportato il canale VOLUME_KMSCLIENT. Questo indica che si tratta effettivamente di un contratto multilicenza.

![Immagine che mostra i risultati di slmgr /dlv, con le informazioni sul canale evidenziate](./media/032618_1700_Troubleshoo14.png)

Che cosa indica quindi il canale RETAIL? Questa dicitura significa che il supporto usato per installare il sistema operativo è un'immagine ISO MSDN. Ho quindi chiesto al mio cliente se per caso sulla rete fosse presente una seconda immagine ISO di Windows Server 2016. Risposta affermativa. Si dà il caso che sulla rete fosse presente un'altra immagine ISO, che era stata usata per creare l'altra decina di computer. Sono state confrontate le due immagini ISO e, chiaramente, quella che mi era stata assegnata per creare i server virtuali era in realtà un'immagine ISO MSDN. Quest'ultima è stata quindi rimossa dalla rete e ora tutti i server sono attivati, senza che sia più necessario preoccuparsi di problemi di attivazione nelle future build.

Spero che la descrizione di questa esperienza possa essere utile ad altri, consentendo di evitare perdite di tempo in futuro.

Mike
