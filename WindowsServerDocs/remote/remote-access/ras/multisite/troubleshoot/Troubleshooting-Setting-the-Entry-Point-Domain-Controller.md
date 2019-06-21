---
title: Risoluzione dei problemi relativi all'impostazione del controller di dominio dei punti di ingresso
description: Questo argomento fa parte della Guida alla distribuzione di più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b12dd0e8-1d80-4d4b-bb45-586f19d17ef0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e7e09a6715df22882c8a88aedf95a5158dd2a0d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280912"
---
# <a name="troubleshooting-setting-the-entry-point-domain-controller"></a>Risoluzione dei problemi relativi all'impostazione del controller di dominio dei punti di ingresso

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento vengono fornite informazioni sulla risoluzione dei problemi relativi al comando `Set-DAEntryPointDC`. Per assicurarsi che l'errore ricevuto sia correlato all'impostazione del controller di dominio dei punti di ingresso, cercare l'evento con ID 10065 nel registro eventi di Windows.  
  
## <a name="SaveGPOSettings"></a>Salvataggio delle impostazioni server oggetto Criteri di gruppo  
**Errore ricevuto**. Si è verificato un errore durante il salvataggio delle impostazioni di accesso remoto nell'oggetto Criteri di gruppo < nome_oggetto >.  
  
Per risolvere questo errore, vedere il salvataggio delle impostazioni di server oggetto Criteri di gruppo.  
  
## <a name="remote-access-is-not-configured"></a>Accesso remoto non configurato  
**Errore ricevuto**. Accesso remoto non è configurato in < nome_server >. Specificare il nome di un server che fa parte di una distribuzione multisito.  
  
Oppure  
  
Accesso remoto non è configurato nel server di < nome_server >. Specificare un computer in cui DirectAccess è abilitato.  
  
**Causa**  
  
Accesso remoto non è configurato nel computer specificato dal parametro *ComputerName*.  
  
Il cmdlet `Set-DaEntryPointDC` è disponibile solo nei server che fanno parte di una distribuzione multisito configurata.  
  
**Soluzione**  
  
Eseguire il comando e assicurarsi di specificare il parametro *ComputerName* con il nome del server già configurato come parte della distribuzione multisito.  
  
## <a name="multisite-is-not-enabled"></a>Multisito non abilitato  
**Errore ricevuto**. È necessario abilitare una distribuzione multisito prima di eseguire questa operazione. A tale scopo, utilizzare il cmdlet `Enable-DAMultiSite`.  
  
**Causa**  
  
Nel server specificato dal parametro *ComputerName* non è abilitata la distribuzione multisito.  
  
Il cmdlet `Set-DaEntryPointDC` è disponibile solo nei server che fanno parte di una distribuzione multisito configurata.  
  
**Soluzione**  
  
Eseguire il comando e assicurarsi di specificare il parametro *ComputerName* con il nome del server già configurato come parte della distribuzione multisito.  
  
## <a name="entry-point-and-domain-controller-not-provided-in-cmdlet"></a>Punto di ingresso e controller di dominio non specificati nel cmdlet  
Il cmdlet `Set-DaEntryPointDC` consente di modificare il controller di dominio associato a punti di ingresso diversi, ad esempio nel caso non sia più disponibile un particolare controller di dominio. È possibile aggiornare un punto di ingresso specifico per utilizzare un controller di dominio diverso oppure aggiornare tutti i punti di ingresso che utilizzano uno specifico controller di dominio per l'utilizzo di un nuovo controller di dominio. Nel primo caso, è consigliabile utilizzare il parametro *EntryPointName* per specificare il punto di ingresso da aggiornare. Nel secondo caso, è necessario utilizzare il parametro *ExistingDC* per specificare il controller di dominio da sostituire. È possibile specificare uno solo di questi parametri.  
  
**Errore ricevuto**. Senza parametri obbligatori sono stati specificati. Specificare il nome di un punto di ingresso o di un controller di dominio esistente.  
  
Oppure  
  
Nel cmdlet `Set-DaEntryPointDC` mancano tutti i parametri obbligatori.  
  
**Causa**  
  
I parametri *EntryPointName* o *ExistingDC* non sono stati specificati oppure sono stati specificati entrambi per il cmdlet `Set-DaEntryPointDC`.  
  
**Soluzione**  
  
Eseguire il comando e assicurarsi di specificare il parametro *EntryPointName* o il parametro *ExistingDC*.  
  
## <a name="could-not-locate-domain-controller"></a>Impossibile individuare il controller di dominio  
**Errore ricevuto**. Impossibile individuare automaticamente un nuovo controller di dominio. Riprovare più tardi o verificare le impostazioni del controller di dominio.  
  
**Causa**  
  
Il computer specificato con il parametro *ComputerName* non è raggiungibile tramite RPC oppure il dominio non contiene controller di dominio scrivibili disponibili.  
  
**Soluzione**  
  
Assicurarsi che il computer remoto sia accessibile tramite RPC e che sia disponibile un controller di dominio scrivibile per il dominio. Se per il dominio è disponibile un controller di dominio scrivibile, è inoltre possibile specificarne il nome in modo esplicito tramite il parametro *NewDC*.  
  
## <a name="could-not-connect-to-domain-controller"></a>Impossibile connettersi al controller di dominio  
  
-   **Problema 1**  
  
    **Errore ricevuto**. Impossibile raggiungere il controller di dominio < controller_dominio >. Controllare la connettività di rete e la disponibilità del server.  
  
    **Causa**  
  
    Non è possibile raggiungere il controller di dominio. Ciò si verifica solo quando l'amministratore specifica un controller di dominio nei parametri *NewDC* o *ExistingDC*.  
  
    **Soluzione**  
  
    Assicurarsi che il nome del controller di dominio sia digitato correttamente. Se per specificare il nome è stato utilizzato un nome breve, utilizzare l'FQDN e riprovare.  
  
-   **Problema 2**  
  
    **Errore ricevuto**. Impossibile contattare il controller di dominio < controller_dominio >.  
  
    **Causa**  
  
    Potrebbe esistere un problema di rete che indica l'impossibilità di raggiungere il controller di dominio specificato nel parametro *NewDC* o qualsiasi altro controller di dominio esistente nella configurazione.  
  
    **Soluzione**  
  
    Assicurarsi che il nome del controller di dominio sia digitato in modo corretto, verificare che esista, sia in esecuzione, sia scrivibile e che esista una relazione di trust tra il controller di dominio e il dominio.  
  
-   **Problema 3**  
  
    **Errore ricevuto**. Impossibile raggiungere il controller di dominio < controller_dominio > per %2 2!s!.  
  
    **Causa**  
  
    Per mantenere la coerenza di configurazione in una distribuzione multisito, è importante assicurarsi che ogni oggetto Criteri di gruppo sia gestito da un singolo controller di dominio. Quando il controller di dominio che gestisce server di un punto di ingresso oggetto Criteri di gruppo non è disponibile, non è possibile leggere o modificare le impostazioni di configurazione di accesso remoto.  
  
    **Soluzione**  
  
    Seguire la procedura "per modificare il controller di dominio che gestisce oggetti Criteri di gruppo di server" descritta in [2.4. Configurare oggetti Criteri di gruppo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
-   **Problema 4**  
  
    **Errore ricevuto**. Impossibile raggiungere il controller di dominio primario nel dominio < nome_dominio >.  
  
    **Causa**  
  
    Per mantenere la coerenza di configurazione in una distribuzione multisito, è importante assicurarsi che ogni oggetto Criteri di gruppo sia gestito da un singolo controller di dominio. Gli oggetti Criteri di gruppo client sono gestiti nel controller di dominio primario. Se il controller di dominio primario non è disponibile, le impostazioni di configurazione di Accesso remoto non possono essere lette o modificate.  
  
    **Soluzione**  
  
    Seguire la procedura "per trasferire il ruolo emulatore PDC" descritta in [2.4. Configurare oggetti Criteri di gruppo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
## <a name="read-only-domain-controller"></a>Controller di dominio di sola lettura  
**Errore ricevuto**. Il controller di dominio < controller_dominio > è di sola lettura. Specificare un controller di dominio non di sola lettura.  
  
**Causa**  
  
Il controller di dominio specificato con il parametro *NewDC* è di sola lettura.  
  
**Soluzione**  
  
Quando si utilizza `Set-DAEntryPointDC`, il parametro *NewDC* viene utilizzato per aggiornare il controller di dominio associato a un particolare punto di ingresso o per aggiornare tutti i punti di ingresso associati a un controller di dominio. Il nuovo controller di dominio deve pertanto essere scrivibile. Specificare un controller di dominio scrivibile nel parametro *NewDC* e riprovare.  
  
## <a name="cannot-retrieve-gpo"></a>Impossibile recuperare l'oggetto Criteri di gruppo  
  
-   **Problema 1**  
  
    **Errore ricevuto**. Oggetto Criteri di gruppo < nome_oggetto > in < controller_dominio_precedente > controller di dominio non può essere recuperata dal controller di dominio < controller_dominio_sostitutivo > perché non sono nello stesso dominio.  
  
    **Causa**  
  
    Il server di Accesso remoto e il controller di dominio non sono inclusi nello stesso dominio, quindi non è possibile recuperare l'oggetto Criteri di gruppo.  
  
    **Soluzione**  
  
    Se si è tentato di aggiornare un punto di ingresso specifico, assicurarsi che il nuovo controller di dominio sia nello stesso dominio del server del punto di ingresso. Se si è tentato di aggiornare un controller di dominio specifico, assicurarsi che il nuovo controller di dominio sia nello stesso dominio di quello che si sta tentando di sostituire.  
  
-   **Problema 2**  
  
    **Errore ricevuto**. Oggetto Criteri di gruppo < nome_oggetto > in < controller_dominio_precedente > controller di dominio non può essere recuperata dal controller di dominio < controller_dominio_sostitutivo >. Attendere il completamento della replica del dominio e quindi riprovare.  
  
    **Causa**  
  
    Quando si tenta di aggiornare un controller di dominio dei punti di ingresso, il cmdlet tenta di leggere l'oggetto Criteri di gruppo del server dal nuovo controller di dominio. Non è tuttavia possibile trovare l'oggetto Criteri di gruppo nel nuovo controller di dominio perché non è ancora stato replicato.  
  
    **Soluzione**  
  
    L'oggetto Criteri di gruppo del server non esiste nel nuovo controller di dominio. Assicurarsi che gli oggetti Criteri di gruppo siano stati replicati in modo corretto nel nuovo controller di dominio e riprovare.  
  
-   **Problema 3**  
  
    **Errore ricevuto**. Non hai le autorizzazioni per accedere a oggetto Criteri di gruppo < nome_oggetto >.  
  
    **Causa**  
  
    Quando si tenta di aggiornare un controller di dominio dei punti di ingresso, il cmdlet tenta di leggere l'oggetto Criteri di gruppo del server dal nuovo controller di dominio. Non è tuttavia possibile leggere l'oggetto Criteri di gruppo nel nuovo controller di dominio perché non si dispone delle autorizzazioni corrette.  
  
    **Soluzione**  
  
    L'oggetto Criteri di gruppo esiste nel controller di dominio, ma non è possibile leggerlo. Assicurarsi di disporre delle autorizzazioni necessarie e riprovare.  
  
## <a name="entry-point-not-part-of-multisite-deployment"></a>Il punto di ingresso non fa parte della distribuzione multisito  
**Errore ricevuto**. Punto di ingresso < nome_punto_di_ingresso > non fa parte della distribuzione multisito. Specificare un valore alternativo.  
  
**Causa**  
  
Non è possibile trovare il nome del punto di ingresso specificato.  
  
**Soluzione**  
  
Assicurarsi che il nome del punto di ingresso sia digitato correttamente e che gli oggetti Criteri di gruppo siano replicati nei controller di dominio richiesti, quindi riprovare. Per visualizzare il controller di dominio assegnato per ogni punto di ingresso, utilizzare `Get-DAEntryPointDC`.  
  
## <a name="remote-access-server-settings"></a>Impostazioni del server di Accesso remoto  
  
-   **Problema 1**  
  
    **Errore ricevuto**. Server < server_name > nel punto di ingresso < nome_punto_di_ingresso > non è accessibile.  
  
    **Causa**  
  
    Durante il tentativo di aggiornamento di un controller di dominio dei punti di ingresso, il cmdlet tenta di leggere e scrivere il controller di dominio dei punti di ingresso da tutti i server di Accesso remoto rilevanti. Il cmdlet non è riuscito a leggere i dati da uno o più server di Accesso remoto.  
  
    **Soluzione**  
  
    Assicurarsi che tutti i server di Accesso remoto rilevanti siano in esecuzione e di disporre delle autorizzazioni di amministratore locale in tutti i server, quindi riprovare.  
  
-   **Problema 2**  
  
    **Errore ricevuto**. Impossibile salvare le impostazioni nel Registro di sistema sul server < nome_server > nel punto di ingresso < nome_punto_di_ingresso >.  
  
    **Causa**  
  
    Durante il tentativo di aggiornamento di un controller di dominio dei punti di ingresso, il cmdlet tenta di leggere e scrivere il controller di dominio dei punti di ingresso da tutti i server di Accesso remoto rilevanti. Il cmdlet non è riuscito a scrivere i dati in uno o più server di Accesso remoto.  
  
    **Soluzione**  
  
    Assicurarsi che tutti i server di Accesso remoto rilevanti siano in esecuzione e di disporre delle autorizzazioni di amministratore locale in tutti i server, quindi riprovare.  
  
-   **Problema 3**  
  
    **Errore ricevuto**. Impossibile applicare gli aggiornamenti di oggetto Criteri di gruppo su < nome_server >. Le modifiche verranno applicate al prossimo aggiornamento dei criteri.  
  
    **Causa**  
  
    Quando si utilizza il cmdlet `Set-DAEntryPointDC`, il parametro *ComputerName* specificato è un server di Accesso remoto in un punto di ingresso diverso dall'ultimo aggiunto alla distribuzione multisito.  
  
    **Soluzione**  
  
    Qualsiasi server non aggiornato può essere visualizzato tramite **Stato configurazione** in **DASHBOARD** della Console di gestione Accesso remoto. Questa situazione non causa problemi funzionali. È comunque possibile eseguire `gpupdate /force` in qualsiasi server non aggiornato per recuperare immediatamente lo stato della configurazione.  
  
## <a name="problem-resolving-fqdn"></a>Problema durante la risoluzione del nome di dominio completo  
**Errore ricevuto**. Server < server_name > nel punto di ingresso < nome_punto_di_ingresso > non è accessibile.  
  
**Causa**  
  
Durante il recupero dell'elenco dei server DirectAccess da modificare, il cmdlet non è riuscito a risolvere il nome di dominio completo (FQDN) di uno dei server dal relativo SID del computer.  
  
**Soluzione**  
  
Il punto di ingresso specificato nel messaggio di errore è associato a un controller di dominio. Assicurarsi che il controller di dominio sia disponibile per il punto di ingresso. Se il computer a cui appartiene il SID specificato è stato rimosso dal dominio, ignorare il messaggio e quindi rimuovere il server dalla distribuzione multisito.  
  
## <a name="no-entry-points-to-update"></a>Nessun punto di ingresso da aggiornare  
**Avviso ricevuto**. Non sono state modificate le impostazioni di controller di dominio. Se è necessario modificarle, verificare che i parametri del cmdlet siano configurati correttamente e che gli oggetti Criteri di gruppo siano stati replicati nei controller di dominio necessari.  
  
**Causa**  
  
Durante la chiama del cmdlet `Set-DaEntryPointDC` con il parametro *ExistingDC*, DirectAccess controlla tutti i punti di ingresso e aggiorna quelli associati al controller di dominio specificato. Nessun punto di ingresso, tuttavia, utilizza il controller *ExistingDC*.  
  
**Soluzione**  
  
Per visualizzare l'elenco dei punti di ingresso e dei controller di dominio associati, utilizzare il cmdlet `Get-DAEntryPointDC`. Se è previsto che vengano apportate modifiche, assicurarsi che i parametri del cmdlet siano digitati correttamente e che gli oggetti Criteri di gruppo siano replicati nei controller di dominio richiesti, quindi riprovare.  
  


