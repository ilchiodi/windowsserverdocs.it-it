---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: Verificare le funzionalità DNS per supportare la replica di directory
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: identity-adds
ms.author: joflore
ms.date: 05/31/2017
ms.tgt_pltfrm: na
author: Femila
ms.openlocfilehash: 066f7ebe1cd8f981344e059797daa9d3f86bdf49
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409059"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>Verificare le funzionalità DNS per supportare la replica di directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Per controllare le impostazioni di Domain Name System (DNS) che potrebbero interferire con la replica Active Directory, è possibile iniziare eseguendo il test di base che garantisce che il DNS funzioni correttamente per il dominio. Dopo aver eseguito il test di base, è possibile testare altri aspetti della funzionalità DNS, inclusa la registrazione dei record di risorse e l'aggiornamento dinamico.

Sebbene sia possibile eseguire questo test della funzionalità DNS di base in qualsiasi controller di dominio, in genere si esegue questo test sui controller di dominio che si ritiene possano riscontrare problemi di replica, ad esempio controller di dominio che segnalano gli ID evento 1844, 1925, 2087 o 2088 nel log DNS del servizio directory Visualizzatore eventi.



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>Esecuzione del test DNS di base del controller di dominio @ no__t-0

Il test DNS di base controlla gli aspetti seguenti della funzionalità DNS:


- **Connettività** Il test determina se i controller di dominio sono registrati in DNS, possono essere contattati dal comando <system>ping</system> e avere connettività Lightweight Directory Access Protocol/Remote Procedure Call (LDAP/RPC). Se il test di connettività non riesce in un controller di dominio, non vengono eseguiti altri test sul controller di dominio. Il test di connettività viene eseguito automaticamente prima dell'esecuzione di qualsiasi altro test DNS.
- **Servizi essenziali:** Il test conferma che i servizi seguenti sono in esecuzione e sono disponibili nel controller di dominio testato: Il servizio client DNS, il servizio Accesso rete, il servizio Centro distribuzione chiavi (KDC) e il servizio server DNS (se DNS è installato nel controller di dominio).
- **Configurazione client DNS:**  Il test conferma che i server DNS in tutte le schede di rete del computer client DNS sono raggiungibili.
- **Registrazioni di record di risorse:** Il test conferma che il record di risorse host (A) di ogni controller di dominio è registrato in almeno uno dei server DNS configurati nel computer client.
- **Zona e inizio dell'autorità (SOA):** Se il controller di dominio esegue il servizio server DNS, il test conferma che sono presenti la Active Directory zona di dominio e il record di risorse di autorità SOA (SOA) per la zona del dominio Active Directory.
- **Zona radice:** Verifica se è presente la zona radice (.).

L'appartenenza al gruppo Enterprise Admins o a un gruppo equivalente è il requisito minimo necessario per completare queste procedure.

È possibile utilizzare la procedura seguente per verificare la funzionalità DNS di base.
     
### <a name="to-verify-basic-dns-functionality"></a>Per verificare la funzionalità DNS di base:


1. Nel controller di dominio che si desidera testare o in un computer membro di dominio in cui sono installati gli strumenti di Active Directory Domain Services (AD DS), aprire un prompt dei comandi come amministratore. Per aprire un prompt dei comandi come amministratore, fare clic sul pulsante **Start**. 
2. In Inizia ricerca digitare prompt dei comandi. 
3. Nella parte superiore del menu Start, fare clic con il pulsante destro del mouse su prompt dei comandi e quindi scegliere Esegui come amministratore. Se viene visualizzata la finestra di dialogo Controllo account utente, verificare che l'azione visualizzata sia quella desiderata, quindi scegliere Continua.
4. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO: `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>Sostituire il nome distinto effettivo, il nome NetBIOS o il nome DNS del controller di dominio per &lt;DCName @ no__t-1. In alternativa, è possibile testare tutti i controller di dominio nella foresta digitando/e: anziché/s:. L'opzione/f specifica un nome di file, che nel comando precedente è dcdiagreport. txt. Se si vuole inserire il file in un percorso diverso dalla directory di lavoro corrente, è possibile specificare un percorso di file, ad esempio/f: c:reportsdcdiagreport.txt.

5. Aprire il file dcdiagreport. txt nel blocco note o in un editor di testo simile. Per aprire il file nel blocco note, digitare Notepad dcdiagreport. txt al prompt dei comandi e quindi premere INVIO. Se il file è stato inserito in una directory di lavoro diversa, includere il percorso del file. Ad esempio, se il file è stato inserito in c:Reports, digitare notepad c:reportsdcdiagreport.txt e quindi premere INVIO.
6. Scorrere fino alla tabella di riepilogo nella parte inferiore del file. 
</br></br>Prendere nota dei nomi di tutti i controller di dominio che segnalano lo stato "avviso" o "errore" nella tabella di riepilogo.  Provare a determinare se è presente un problema di controller di dominio individuando la sezione dettagliata di breakout cercando la stringa "DC: DCName, dove DCName è il nome effettivo del controller di dominio.

Se vengono visualizzate le modifiche di configurazione necessarie, impostarle in base alle esigenze. Se, ad esempio, si nota che uno dei controller di dominio ha un indirizzo IP ovviamente errato, è possibile correggerlo. Quindi eseguire di nuovo il test.

Per convalidare le modifiche alla configurazione, eseguire di nuovo il comando dcdiag/test: DNS/v con l'opzione/e: o/s:, a seconda dei casi. Se nel controller di dominio non è abilitato IP versione 6 (IPv6), è necessario aspettarsi che la parte di convalida dell'host (AAAA) del test abbia esito negativo, ma se non si utilizza IPv6 nella rete, questi record non sono necessari.
            
## <a name="verifying-resource-record-registration"></a>Verifica della registrazione del record di risorse
    
Il controller di dominio di destinazione usa il record di risorse alias DNS (CNAME) per individuare il partner di replica del controller di dominio di origine. Sebbene i controller di dominio che eseguono Windows Server (a partire da Windows Server 2003 con Service Pack 1 (SP1)) possano individuare i partner di replica di origine usando nomi di dominio completi (FQDN) o, in caso di errore, NetBIOS namesthe la presenza dell'alias (CNAME) il record di risorse è previsto ed è necessario verificarne il corretto funzionamento DNS. 
      
È possibile utilizzare la procedura seguente per verificare la registrazione dei record di risorse, inclusa la registrazione dei record di risorse alias (CNAME).
      
### <a name="to-verify-resource-record-registrationtitle"></a>Per verificare la registrazione del record di risorse @ no__t-0


1. Apri il prompt dei comandi come amministratore. Per aprire un prompt dei comandi come amministratore, fare clic su Avvia. In Inizia ricerca digitare prompt dei comandi. 
2. Nella parte superiore del menu Start, fare clic con il pulsante destro del mouse su prompt dei comandi e quindi scegliere Esegui come amministratore. Se viene visualizzata la finestra di dialogo Controllo account utente, verificare che l'azione visualizzata sia quella desiderata, quindi scegliere Continua.  </br></br>È possibile utilizzare lo strumento Dcdiag per verificare la registrazione di tutti i record di risorse essenziali per il percorso del controller di dominio eseguendo il comando `dcdiag /test:dns /DnsRecordRegistration`.

Questo comando verifica la registrazione dei seguenti record di risorse in DNS:


- **alias (CNAME):** record di risorse basato sull'identificatore univoco globale (Guid) che individua un partner di replica
- **host (A):** record di risorse host che contiene l'indirizzo IP del controller di dominio
- **LDAP SRV:** record di risorse del servizio (SRV) che individuano i server LDAP
- **GC SRV**: record di risorse del servizio (SRV) che individuano i server di catalogo globale
- **PDC SRV**: record di risorse del servizio (SRV) che individuano i master operazioni dell'emulatore del controller di dominio primario (PDC)

È possibile utilizzare la procedura seguente per verificare la registrazione dei record di risorse alias (CNAME).

### <a name="to-verify-alias-cname-resource-record-registration"></a>Per verificare la registrazione del record di risorse alias (CNAME)

1. Aprire lo snap-in DNS. Per aprire DNS, fare clic su Avvia. In Inizia ricerca digitare dnsmgmt. msc, quindi premere INVIO. Se viene visualizzata la finestra di dialogo controllo account utente, verificare che venga visualizzata l'azione desiderata, quindi fare clic su continua.
2. Utilizzare lo snap-in DNS per individuare qualsiasi controller di dominio in cui è in esecuzione il servizio server DNS, in cui il server ospita la zona DNS con lo stesso nome del dominio Active Directory del controller di dominio.
3. Nell'albero della console fare clic sulla zona denominata _msdcs. Dns_Domain_Name.
4. Nel riquadro dei dettagli verificare che siano presenti i record di risorse seguenti: un record di risorse alias (CNAME) denominato Dsa_Guid. _msdcs. <placeholder>Dns_Domain_Name</placeholder> e un record di risorse host (a) corrispondente per il nome del server DNS.

Se il record di risorse alias (CNAME) non è registrato, verificare che aggiornamento dinamico funzioni correttamente. Usare il test nella sezione seguente per verificare l'aggiornamento dinamico.
    
## <a name="verifying-dynamic-update"></a>Verifica dell'aggiornamento dinamico
    
Se il test DNS di base mostra che i record di risorse non esistono in DNS, utilizzare il test di aggiornamento dinamico per determinare il motivo per cui il servizio Accesso rete non ha registrato automaticamente i record di risorse. Per verificare che la zona del dominio Active Directory sia configurata in modo da accettare aggiornamenti dinamici protetti e per eseguire la registrazione di un record di test (_dcdiag_test_record), utilizzare la procedura riportata di seguito. Il record di test viene eliminato automaticamente dopo il test.

### <a name="to-verify-dynamic-updatetitle"></a>Per verificare l'aggiornamento dinamico @ no__t-0


1. Apri il prompt dei comandi come amministratore. Per aprire un prompt dei comandi come amministratore, fare clic su Avvia. In Inizia ricerca digitare prompt dei comandi. Nella parte superiore del menu Start, fare clic con il pulsante destro del mouse su prompt dei comandi e quindi scegliere Esegui come amministratore. Se viene visualizzata la finestra di dialogo Controllo account utente, verificare che l'azione visualizzata sia quella desiderata, quindi scegliere Continua.
2. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO: `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>Sostituire il nome distinto, il nome NetBIOS o il nome DNS del controller di dominio per &lt;DCName @ no__t-1. In alternativa, è possibile testare tutti i controller di dominio nella foresta digitando/e: anziché/s:. Se per il controller di dominio non è abilitato IPv6, è necessario prevedere che la parte del record di risorse dell'host (AAAA) del test abbia esito negativo, situazione normale quando IPv6 non è abilitato.

Se gli aggiornamenti dinamici protetti non sono configurati, è possibile utilizzare la procedura seguente per configurarli.

### <a name="to-enable-secure-dynamic-updates"></a>Per abilitare gli aggiornamenti dinamici protetti


1. Aprire lo snap-in DNS. Per aprire DNS, fare clic su Avvia. 
2. In Inizia ricerca digitare dnsmgmt. msc, quindi premere INVIO. Se viene visualizzata la finestra di dialogo controllo account utente, verificare che venga visualizzata l'azione desiderata, quindi fare clic su continua.
3. Nell'albero della console fare clic con il pulsante destro del mouse sulla zona applicabile, quindi scegliere Proprietà.
4. Nella scheda Generale verificare che il tipo di zona sia Active Directory-Integrated.
5. In aggiornamenti dinamici fare clic su solo protetto.

## <a name="registering-dns-resource-records"></a>Registrazione di record di risorse DNS
    
Se i record di risorse DNS non vengono visualizzati nel DNS per il controller di dominio di origine, sono stati verificati aggiornamenti dinamici e si desidera registrare i record di risorse DNS immediatamente, è possibile forzare la registrazione manualmente usando la procedura seguente. Il servizio Accesso rete in un controller di dominio registra i record di risorse DNS necessari per la posizione del controller di dominio nella rete. Il servizio client DNS registra il record di risorse host (A) a cui punta il record alias (CNAME).

### <a name="to-register-dns-resource-records-manuallytitle"></a>Per registrare manualmente i record di risorse DNS @ no__t-0


1. Apri il prompt dei comandi come amministratore. Per aprire un prompt dei comandi come amministratore, fare clic su Avvia. 
2. In Inizia ricerca digitare prompt dei comandi. 
3. Nella parte superiore del menu Start, fare clic con il pulsante destro del mouse su prompt dei comandi e quindi scegliere Esegui come amministratore. Se viene visualizzata la finestra di dialogo Controllo account utente, verificare che l'azione visualizzata sia quella desiderata, quindi scegliere Continua.
4. Per avviare manualmente la registrazione dei record di risorse Locator del controller di dominio nel controller di dominio di origine, al prompt dei comandi digitare il comando seguente e quindi premere INVIO: `net stop netlogon && net start netlogon`
5. Per avviare manualmente la registrazione del record di risorse host (A), al prompt dei comandi digitare il comando seguente e quindi premere INVIO: `ipconfig /flushdns && ipconfig /registerdns`
6. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO: `dcdiag /test:dns /v /s:<DCName>` </br></br>Sostituire il nome distinto, il nome NetBIOS o il nome DNS del controller di dominio per &lt;DCName @ no__t-1. Esaminare l'output del test per assicurarsi che i test DNS siano stati superati. Se per il controller di dominio non è abilitato IPv6, è necessario prevedere che la parte del record di risorse dell'host (AAAA) del test abbia esito negativo, situazione normale quando IPv6 non è abilitato.
