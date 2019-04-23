---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: Verificare le funzionalità DNS per supportare la replica di directory
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: identity-adds
ms.author: joflore
ms.date: 05/31/2017
ms.tgt_pltfrm: na
author: Femila
ms.openlocfilehash: a55b95ee516abda8bdbae6e9829a161ef060012e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871872"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>Verificare le funzionalità DNS per supportare la replica di directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Per controllare le impostazioni di sistema DNS (Domain Name) che potrebbero interferire con la replica di Active Directory, è possibile iniziare eseguendo il test di base che assicura che DNS funziona correttamente per il dominio. Dopo aver eseguito il test di base, è possibile testare altri aspetti della funzionalità DNS, tra cui la registrazione dei record di risorse e l'aggiornamento dinamico.

Sebbene sia possibile eseguire il test della funzionalità di base DNS in qualsiasi controller di dominio, in genere è eseguito il test controller di dominio che si ritiene potrebbero verificarsi problemi di replica, ad esempio, i controller di dominio che segnalano eventi ID 1844, 1925, 2087, o 2088 nel log DNS del servizio Directory di Visualizzatore eventi.



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>Esegue il controller di dominio base test DNS</title>

Il test di base DNS controlla i seguenti aspetti della funzionalità DNS:


- **Connettività:** Il test determina se sono presenti controller registrati nel servizio DNS di dominio possa essere contattata dal <system>ping</system> comando e avere Lightweight Directory Access Protocol / connettività LDAP/RPC () di chiamata di procedura remota. Se il test della connettività ha esito negativo in un controller di dominio, nessun altro test vengono eseguiti su quel controller di dominio. Il test della connettività viene eseguito automaticamente prima di eseguita qualsiasi altro test DNS.
- **Servizi essenziali:** Il test conferma che i servizi seguenti siano in esecuzione e disponibili nel controller di dominio testato: Servizio Client DNS, servizio Accesso rete, servizio Centro distribuzione chiavi (KDC) e il servizio Server DNS (se DNS è installato nel controller di dominio).
- **Configurazione del client DNS:**  Il test conferma che i server DNS su tutte le schede di rete del computer client DNS siano raggiungibili.
- **Registrazioni dei record di risorse:** Il test conferma che il record di risorse host (A) di ogni controller di dominio è registrato in almeno uno dei server DNS configurato nel computer client.
- **Zona e l'avvio di autorità (SOA):** Se il controller di dominio è in esecuzione il servizio Server DNS, il test conferma che la zona del dominio Active Directory e l'inizio del record di risorse di autorità (SOA) per la zona del dominio Active Directory siano presenti.
- **Zona radice:** Controlla se la zona radice (.) è presenta.

L'appartenenza al gruppo Enterprise Admins, o equivalente è il requisito minimo necessario per completare queste procedure.

È possibile utilizzare la procedura seguente per verificare la funzionalità di base DNS.
     
### <a name="to-verify-basic-dns-functionality"></a>Per verificare la funzionalità di base DNS:


1. Nel controller di dominio che si desidera testare o in un computer membro di dominio che è installati gli strumenti di Active Directory Domain Services (AD DS), aprire un prompt dei comandi come amministratore. Per aprire un prompt dei comandi come amministratore, fare clic sul pulsante **Start**. 
2. In Inizia ricerca, digitare il prompt dei comandi. 
3. Nella parte superiore del menu Start, fare doppio clic su prompt dei comandi e quindi fare clic su Esegui come amministratore. Se viene visualizzata la finestra di dialogo Controllo account utente, verificare che l'azione visualizzata sia quella desiderata, quindi scegliere Continua.
4. Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO: `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>Sostituire con l'effettivo nome distinto, un nome NetBIOS o un nome DNS del controller di dominio per &lt;DCName&gt;. In alternativa, è possibile testare tutti i controller di dominio nella foresta digitando /e: anziché /s:. L'opzione /f Specifica un nome di file, ovvero nel comando precedente dcdiagreport.txt. Se si desidera inserire il file in un percorso diverso dalla directory di lavoro corrente, è possibile specificare un percorso di file, ad esempio /f:c:reportsdcdiagreport.txt.

5. Aprire il file dcdiagreport.txt nel blocco note o in un editor di testo simili. Per aprire il file nel blocco note, al prompt dei comandi, digitare dcdiagreport.txt blocco note e quindi premere INVIO. Se si ha inserito il file in una directory di lavoro diverso, includere il percorso del file. Ad esempio, se il file è stato inserito in c:reports, digitare c:reportsdcdiagreport.txt blocco note e quindi premere INVIO.
6. Scorrere fino alla tabella di riepilogo nella parte inferiore del file. 
</br></br>Notare i nomi di tutti i controller di dominio che segnalano lo stato "Avviso" o "Difettoso" nella tabella di riepilogo.  Provare a determinare se è disponibile un controller di dominio problema trovando la sezione di scomposizione dettagliata eseguendo una ricerca per la stringa "controller di dominio: DCName", dove DCName è il nome effettivo del controller di dominio.

Qualora notassi modifiche di configurazione ovvio che sono necessari, renderli, come appropriato. Ad esempio, se si nota che uno dei controller di dominio ha un indirizzo IP ovviamente non corretto, è possibile correggerlo. Quindi, eseguire di nuovo il test.

Per convalidare le modifiche alla configurazione, eseguire nuovamente il comando di /v Dcdiag /test: DNS con l'opzione /s: o /e:, come appropriato. Se non hai IP versione 6 (IPv6) abilitato sul controller di dominio, è necessario prevedere che la parte di convalida host (AAAA) di riuscita del test, ma se non si usa IPv6 nella rete, questi record non sono necessari.
            
## <a name="verifying-resource-record-registration"></a>Verifica per determinare se la registrazione dei record di risorse
    
Il controller di dominio di destinazione Usa il record di risorse alias (CNAME) DNS per individuare il partner di replica controller di dominio di origine. Anche se i controller di dominio che esegue Windows Server (a partire da Windows Server 2003 con Service Pack 1 (SP1)) possono individuare i partner di replica di origine usando nomi di dominio completo (FQDN) o, se il problema persiste, NetBIOS namesthe presenza dell'alias (CNAME) record di risorse è prevista e devono essere verificati per DNS di corretto funzionamento. 
      
È possibile utilizzare la procedura seguente per verificare la registrazione dei record di risorse, tra cui la registrazione dei record di risorse alias (CNAME).
      
### <a name="to-verify-resource-record-registrationtitle"></a>Per verificare la registrazione dei record di risorse</title>


1. Apri il prompt dei comandi come amministratore. Per aprire un prompt dei comandi come amministratore, fare clic su Avvia. In Inizia ricerca, digitare il prompt dei comandi. 
2. Nella parte superiore del menu Start, fare doppio clic su prompt dei comandi e quindi fare clic su Esegui come amministratore. Se viene visualizzata la finestra di dialogo Controllo account utente, verificare che l'azione visualizzata sia quella desiderata, quindi scegliere Continua.  </br></br>È possibile usare lo strumento Dcdiag per verificare la registrazione di tutti i record di risorse che sono essenziali per l'individuazione dei controller di dominio eseguendo il `dcdiag /test:dns /DnsRecordRegistration` comando.

Questo comando verifica la registrazione dei record di risorse seguenti nel DNS:


- **alias (CNAME):** l'identificatore univoco globale (GUID)-basato su record di risorse che individua un partner di replica
- **host (A):** il record di risorse host contenente l'indirizzo IP del controller di dominio
- **LDAP SRV:** il record di risorse del servizio (SRV) individuare i server LDAP
- **GC SRV**: il record di risorse del servizio (SRV) individuare globale dei server di catalogo
- **PDC SRV**: il record di risorse del servizio (SRV) individuare master operazioni di dominio primario (PDC) controller emulatore

È possibile utilizzare la procedura seguente per verificare la registrazione di record risorse alias (CNAME) da solo.

### <a name="to-verify-alias-cname-resource-record-registration"></a>Per verificare la registrazione dei record di risorse alias (CNAME)

1. Aprire lo snap-in DNS. Per aprire DNS, fare clic su Avvia. In Inizia ricerca, digitare dnsmgmt. msc e quindi premere INVIO. Se viene visualizzata la finestra di dialogo controllo dell'Account utente, verificare che venga visualizzata l'azione si desidera e quindi fare clic su Continua.
2. Utilizzare lo snap-in DNS per individuare eventuali controller di dominio che esegue il servizio Server DNS, in cui il server ospita la zona DNS con lo stesso nome di dominio di Active Directory del controller di dominio.
3. Nell'albero della console, fare clic sulla zona msdcs denominato. Dns_Domain_Name.
4. Nel riquadro dei dettagli verificare che siano presenti i seguenti record di risorse: un record di risorse alias (CNAME) denominato Dsa_Guid._msdcs. <placeholder>Dns_Domain_Name</placeholder> e un corrispondente (A) resource record host per il nome del server DNS.

Se il record di risorse alias (CNAME) non è registrato, verificare che l'aggiornamento dinamico funzionino correttamente. Utilizzare il test nella sezione seguente per verificare l'aggiornamento dinamico.
    
## <a name="verifying-dynamic-update"></a>Verifica dell'aggiornamento dinamico
    
Se il test di base DNS mostra che i record di risorse non esiste nel DNS, usare il test di aggiornamento dinamico per determinare il motivo per cui il servizio Accesso rete non è stato registrato il record di risorse automaticamente. Per verificare che la zona del dominio Active Directory sia configurata per accettare aggiornamenti dinamici protetti e per eseguire la registrazione di un record di prova (_dcdiag_test_record), usare la procedura seguente. Il record di prova viene eliminato automaticamente dopo il test.

### <a name="to-verify-dynamic-updatetitle"></a>Per verificare l'aggiornamento dinamico</title>


1. Apri il prompt dei comandi come amministratore. Per aprire un prompt dei comandi come amministratore, fare clic su Avvia. In Inizia ricerca, digitare il prompt dei comandi. Nella parte superiore del menu Start, fare doppio clic su prompt dei comandi e quindi fare clic su Esegui come amministratore. Se viene visualizzata la finestra di dialogo Controllo account utente, verificare che l'azione visualizzata sia quella desiderata, quindi scegliere Continua.
2. Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO: `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>Sostituire con il nome distinto, un nome NetBIOS o un nome DNS del controller di dominio per &lt;DCName&gt;. In alternativa, è possibile testare tutti i controller di dominio nella foresta digitando /e: anziché /s:. Se non hai IPv6 abilitato sul controller di dominio, è necessario prevedere che l'host (AAAA) record parte della risorsa l'esito negativo, ovvero una condizione normale quando IPv6 non è abilitato.

Se non sono configurati gli aggiornamenti dinamici protetti, è possibile utilizzare la procedura seguente per configurarli.

### <a name="to-enable-secure-dynamic-updates"></a>Per abilitare gli aggiornamenti dinamici protetti


1. Aprire lo snap-in DNS. Per aprire DNS, fare clic su Avvia. 
2. In Inizia ricerca, digitare dnsmgmt. msc e quindi premere INVIO. Se viene visualizzata la finestra di dialogo controllo dell'Account utente, verificare che venga visualizzata l'azione desiderato e quindi fare clic su Continua.
3. Nell'albero della console, fare doppio clic sulla zona e quindi fare clic su proprietà.
4. Nella scheda Generale verificare che il tipo di zona è integrato in Active Directory.
5. Negli aggiornamenti dinamici, fare clic su Proteggi solo.

## <a name="registering-dns-resource-records"></a>La registrazione di record di risorse DNS
    
Se i record risorsa DNS non sono presenti nel sistema DNS per il controller di dominio di origine, significa che gli aggiornamenti dinamici e si desidera registrare record di risorse DNS immediatamente, è possibile forzare manualmente la registrazione usando la procedura seguente. Il servizio Accesso rete in un controller di dominio registra record di risorse DNS necessari per il controller di dominio da individuare nella rete. Il servizio Client DNS registra il record di risorse host (A) che fa riferimento il record alias (CNAME).

### <a name="to-register-dns-resource-records-manuallytitle"></a>Per registrare manualmente i record risorsa DNS</title>


1. Apri il prompt dei comandi come amministratore. Per aprire un prompt dei comandi come amministratore, fare clic su Avvia. 
2. In Inizia ricerca, digitare il prompt dei comandi. 
3. Nella parte superiore di avvio, fare doppio clic su prompt dei comandi e quindi fare clic su Esegui come amministratore. Se viene visualizzata la finestra di dialogo Controllo account utente, verificare che l'azione visualizzata sia quella desiderata, quindi scegliere Continua.
4. Per avviare la registrazione della risorsa del localizzatore di controller dominio record manualmente nel controller di dominio di origine, al prompt dei comandi, digitare il comando seguente e quindi premere INVIO: `net stop netlogon && net start netlogon`
5. Per avviare la registrazione dell'host (A) record di risorse manualmente, al prompt dei comandi, digitare il comando seguente e quindi premere INVIO: `ipconfig /flushdns && ipconfig /registerdns`
6. Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO: `dcdiag /test:dns /v /s:<DCName>` </br></br>Sostituire con il nome distinto, un nome NetBIOS o un nome DNS del controller di dominio per &lt;DCName&gt;. Esaminare l'output del test per assicurarsi che i test DNS passato. Se non hai IPv6 abilitato sul controller di dominio, è necessario prevedere che l'host (AAAA) record parte della risorsa l'esito negativo, ovvero una condizione normale quando IPv6 non è abilitato.
