---
title: Linee guida per la risoluzione dei problemi di attivazione correlati al DNS
description: ''
ms.topic: article
ms.date: 09/10/2019
ms.technology: server-general
ms.assetid: ''
author: Teresa-Motiv
ms.author: v-tea
ms.localizationpriority: medium
ms.openlocfilehash: 3165c926c50c2f91544895e0d328f1dae7424b4a
ms.sourcegitcommit: b7f55949f166554614f581c9ddcef5a82fa00625
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2019
ms.locfileid: "72588033"
---
# <a name="guidelines-for-troubleshooting-dns-related-activation-issues"></a>Linee guida per la risoluzione dei problemi di attivazione correlati al DNS

Potrebbe essere necessario usare alcuni di questi metodi se una o più delle condizioni seguenti sono applicabili:

- Usi supporti per contratti multilicenza e&nbsp;un codice Product Key generico per contratti multilicenza per installare uno dei sistemi operativi seguenti:
   - Windows Server 2019
   - Windows Server 2016
   - Windows Server 2012 R2
   - Windows Server 2012
   - Windows Server 2008 R2
   - Windows Server 2008
   - Windows 10
   - Windows 8.1
   - Windows 8
- L'attivazione guidata non può connettersi a un computer host del Servizio di gestione delle chiavi.

Quando provi ad attivare un sistema client, l'attivazione guidata usa DNS per individuare un computer corrispondente in cui sia in esecuzione il software del Servizio di gestione delle chiavi. Se la procedura guidata esegue una query su DNS e non trova la voce DNS per il computer host del Servizio di gestione delle chiavi, verrà segnalato un errore.   

<a id="list"></a>Esamina l'elenco seguente per trovare un approccio appropriato per le tue esigenze:

- Se non puoi installare un host del Servizio di gestione delle chiavi o non puoi usare l'attivazione del Servizio di gestione delle chiavi, prova la procedura [Sostituire il codice Product Key con un codice MAK](#change-the-product-key-to-an-mak).
- Se devi installare e configurare un host del Servizio di gestione delle chiavi, usa la procedura [Configurare un host del Servizio di gestione delle chiavi per i client a fronte dei quali effettuare l'attivazione](#configure-a-kms-host-for-the-clients-to-activate-against).
- Se il client non è in grado di individuare l'host del Servizio di gestione delle chiavi esistente, usa le procedure seguenti per risolvere i problemi relativi alle configurazioni di routing. Queste procedure sono elencate dalla più semplice alla più complessa.
  - [Verificare la connettività IP di base al server DNS](#verify-basic-ip-connectivity-to-the-dns-server)
  - [Verificare la configurazione dell'host del Servizio di gestione delle chiavi](#verify-the-configuration-of-the-kms-host)  
  - [Determinare il tipo di problema di routing](#determine-the-type-of-routing-issue)
  - [Verificare la configurazione DNS](#verify-the-dns-configuration)
  - [Creare manualmente un record SRV del Servizio di gestione delle chiavi](#manually-create-a-kms-srv-record)
  - [Assegnare manualmente un host del Servizio di gestione delle chiavi a un client del Servizio di gestione delle chiavi](#manually-assign-a-kms-host-to-a-kms-client)
  - [Configurare l'host del Servizio di gestione delle chiavi per la pubblicazione in più domini DNS](#configure-the-kms-host-to-publish-in-multiple-dns-domains)

## <a name="change-the-product-key-to-an-mak"></a>Sostituire il codice Product Key con un codice MAK

Se non puoi installare un host del Servizio di gestione delle chiavi o per qualche altro motivo non puoi usare l'attivazione del Servizio di gestione delle chiavi, sostituisci il codice Product Key con un codice MAK. Se hai scaricato le immagini di Windows da Microsoft Developer Network (MSDN) o da TechNet, i codici di riferimento del prodotto (SKU) elencati sotto i supporti sono in genere supporti per contratti multilicenza e il codice Product Key fornito è un codice MAK.

Per sostituire il codice Product Key con un codice MAK, segui questa procedura:

1. Aprire una finestra del prompt dei comandi con privilegi elevati A tale scopo, premi il tasto WINDOWS + X, fai clic con il pulsante destro del mouse su **Prompt dei comandi** e quindi scegli **Esegui come amministratore**. Se è richiesta una password di amministratore o una conferma, digita la password oppure fornisci una conferma.
2. Al prompt dei comandi, eseguire il comando seguente:
   ```cmd
    slmgr -ipk xxxxx-xxxxx-xxxxx-xxxxx-xxxxx
   ```
   > [!NOTE]
   > Il segnaposto **xxxxx-xxxxx-xxxxx-xxxxx-xxxxx** rappresenta il codice Product Key MAK.  

[Torna all'elenco delle procedure.](#list)

## <a name="configure-a-kms-host-for-the-clients-to-activate-against"></a>Configurare un host del Servizio di gestione delle chiavi per i client a fronte dei quali effettuare l'attivazione

Per l'attivazione del Servizio di gestione delle chiavi è necessario che sia configurato un host del Servizio di gestione delle chiavi per i client a fronte dei quali effettuare l'attivazione. Se nell'ambiente non sono configurati host del Servizio di gestione delle chiavi, installane e attivane uno usando una chiave host del Servizio di gestione delle chiavi appropriata. Dopo aver configurato un computer in rete per ospitare il software del Servizio di gestione delle chiavi, pubblica le impostazioni di Domain Name System (DNS).

Per informazioni sul processo di configurazione dell'host del Servizio di gestione delle chiavi, vedi [Attivare tramite il Servizio di gestione delle chiavi](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-key-management-service-vamt) e [Installare e configurare lo Strumento di gestione dell'attivazione dei contratti multilicenza](https://docs.microsoft.com/windows/deployment/volume-activation/install-configure-vamt).

[Torna all'elenco delle procedure.](#list)

## <a name="verify-basic-ip-connectivity-to-the-dns-server"></a>Verificare la connettività IP di base al server DNS

Verifica la connettività IP di base al server DNS usando il comando ping. A tale scopo, segui questa procedura sia sul client del Servizio di gestione delle chiavi in cui si verifica l'errore che sul computer host del Servizio di gestione delle chiavi:

1. Aprire una finestra del prompt dei comandi con privilegi elevati
1. Al prompt dei comandi, eseguire il comando seguente:
   ```cmd
   ping <DNS_Server_IP_address>
   ```
   > [!NOTE]
   > Se l'output di questo comando non include la frase "Risposta da", esiste un problema di rete o un problema DNS che devi risolvere prima di poter usare le altre procedure descritte in questo articolo. Per altre informazioni su come risolvere i problemi relativi a TCP/IP se non riesci effettuare il ping del server DNS, vedi [Risoluzione avanzata dei problemi relativi a TCP/IP](https://docs.microsoft.com/windows/client-management/troubleshoot-tcpip).

[Torna all'elenco delle procedure.](#list)

## <a name="verify-the-configuration-of-the-kms-host"></a>Verificare la configurazione dell'host del Servizio di gestione delle chiavi

Controlla il Registro di sistema del server host del Servizio di gestione delle chiavi per determinare se sta effettuando la registrazione con DNS. Per impostazione predefinita, un server host del Servizio di gestione delle chiavi registra dinamicamente un record SRV DNS ogni 24 ore. 
> [!IMPORTANT]
> Segui con attenzione la procedura descritta in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756) nel caso in cui si verifichino problemi.  

Per controllare questa impostazione, segui questa procedura:
1. Avviare l'editor del Registro di sistema. A tale scopo, fai clic sul pulsante **Start**, scegli **Esegui**, digita **regedit** e quindi premi INVIO.
1. Individua la sottochiave **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL** e verifica il valore della voce **DisableDnsPublishing**. Questa voce presenta i valori possibili seguenti:
   - **0** o non definito (impostazione predefinita): il server host del Servizio di gestione delle chiavi registra un record SRV ogni 24 ore.
   - **1**: il server host del Servizio di gestione delle chiavi non registra automaticamente i record SRV. Se l'implementazione non supporta gli aggiornamenti dinamici, vedi [Creare manualmente un record SRV del Servizio di gestione delle chiavi](#manually-create-a-kms-srv-record).  
1. Se la voce **DisableDnsPublishing** è mancante, creala (il tipo è DWORD). Se la registrazione dinamica è accettabile, lascia il valore come non definito o impostalo su **0**.

[Torna all'elenco delle procedure.](#list)

## <a name="determine-the-type-of-routing-issue"></a>Determinare il tipo di problema di routing

Puoi usare i comandi seguenti per determinare se si tratta di un problema di risoluzione dei nomi o di un problema di record SRV.  

1. In un client del Servizio di gestione delle chiavi apri una finestra del prompt dei comandi con privilegi elevati.  
1. Al prompt dei comandi esegui il comando seguente:
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > In questo comando <KMS_FQDN> rappresenta il nome di dominio completo (FQDN) del computer host del Servizio di gestione delle chiavi e \<port\> rappresenta la porta TCP usata dal Servizio di gestione delle chiavi.  

   Se questi comandi risolvono il problema, si tratta di un problema di record SRV. Puoi risolverlo usando uno dei comandi documentati nella procedura [Assegnare manualmente un host del Servizio di gestione delle chiavi a un client del Servizio di gestione delle chiavi](#manually-assign-a-kms-host-to-a-kms-client).  

1. Se il problema persiste, esegui questi comandi:
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <IP Address>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > In questo comando \<IP Address\> rappresenta l'indirizzo IP del computer host del Servizio di gestione delle chiavi e \<port\> rappresenta la porta TCP usata dal Servizio di gestione delle chiavi.  

   Se questi comandi risolvono il problema, probabilmente si tratta di un problema di risoluzione dei nomi. Per altre informazioni sulla risoluzione dei problemi, vedi la procedura [Verificare la configurazione DNS](#verify-the-dns-configuration).

1. Se nessuno di questi comandi risolve il problema, controlla la configurazione del firewall del computer. Tutte le comunicazioni di attivazione tra i client del Servizio di gestione delle chiavi e l'host del Servizio di gestione delle chiavi usano la porta TCP 1688. I firewall nel client e nell'host del Servizio di gestione delle chiavi devono consentire la comunicazione sulla porta 1688.

[Torna all'elenco delle procedure.](#list)

## <a name="verify-the-dns-configuration"></a>Verificare la configurazione DNS

>[!NOTE]
> Se non diversamente specificato, segui questa procedura su un client del Servizio di gestione delle chiavi in cui si è verificato l'errore applicabile.

1. Apri una finestra del prompt dei comandi con privilegi elevati.
1. Al prompt dei comandi, eseguire il comando seguente:
   ```cmd
   IPCONFIG /all
   ```
1. Nei risultati del comando prendi nota delle informazioni seguenti:
   - Indirizzo IP assegnato del computer client del Servizio di gestione delle chiavi
   - Indirizzo IP del server DNS primario usato dal computer client del Servizio di gestione delle chiavi
   - Indirizzo IP del gateway predefinito usato dal computer client del Servizio di gestione delle chiavi
   - Elenco di ricerca dei suffissi DNS usato dal computer client del Servizio di gestione delle chiavi
1. Verifica che i record SRV dell'host del Servizio di gestione delle chiavi siano registrati in DNS. A tale scopo, effettuare le operazioni seguenti:  
   1. Aprire una finestra del prompt dei comandi con privilegi elevati
   1. Al prompt dei comandi, eseguire il comando seguente:
      ```cmd
      nslookup -type=all _vlmcs._tcp>kms.txt
      ```
   1. Apri il file KMS.txt generato dal comando. Questo file deve contenere una o più voci simili all'esempio seguente:
       ```
       _vlmcs._tcp.contoso.com SRV service location:
       priority = 0
       weight = 0
       port = 1688 svr hostname = kms-server.contoso.com
       ```
       > [!NOTE]
       > In questo esempio contoso.com rappresenta il dominio dell'host del Servizio di gestione delle chiavi.
      1. Verifica l'indirizzo IP, il nome host, la porta e il dominio dell'host del Servizio di gestione delle chiavi.
      1. Se queste voci **_vlmcs** esistono e contengono i nomi host del Servizio di gestione delle chiavi previsti, passa alla procedura [Assegnare manualmente un host del Servizio di gestione delle chiavi a un client del Servizio di gestione delle chiavi](#manually-assign-a-kms-host-to-a-kms-client).  
      > [!NOTE]
      > Se il comando [**nslookup**](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup) trova l'host del Servizio di gestione delle chiavi, non significa che il client DNS possa trovarlo. Se il comando **nslookup** trova l'host del Servizio di gestione delle chiavi, ma non puoi ancora eseguire l'attivazione usando l'host del Servizio di gestione delle chiavi, controlla le altre impostazioni DNS, ad esempio il suffisso DNS primario e l'elenco di ricerca del suffisso DNS.
1. Verifica che l'elenco di ricerca del suffisso DNS primario includa il suffisso di dominio DNS associato all'host del Servizio di gestione delle chiavi. Se l'elenco di ricerca non include queste informazioni, passa alla procedura [Configurare l'host del Servizio di gestione delle chiavi per la pubblicazione in più domini DNS](#configure-the-kms-host-to-publish-in-multiple-dns-domains).

[Torna all'elenco delle procedure.](#list)

## <a name="manually-create-a-kms-srv-record"></a>Creare manualmente un record SRV del Servizio di gestione delle chiavi

Per creare manualmente un record SRV per un host del Servizio di gestione delle chiavi che usa un server DNS Microsoft, segui questa procedura:

1. Nel server DNS aprire Gestore DNS. Per aprire Gestore DNS, fai clic sul pulsante **Start**, scegli **Strumenti di amministrazione** e quindi **DNS**.
1. Seleziona il server DNS in cui devi creare il record di risorse SRV.
1. Nell'albero della console espandi **Zone di ricerca diretta**, fai clic con il pulsante destro del mouse sul dominio e quindi scegli **Altri nuovi record**.
1. Scorri l'elenco verso il basso, seleziona **Posizione servizio (SRV)** e quindi **Crea record**.
1. Digita le informazioni seguenti:
   - Servizio: **_VLMCS**
   - Protocollo: **_TCP**
   - Numero porta: **1688**
   - Host che offre questo servizio: **&lt;*FQDN dell'host del Servizio di gestione delle chiavi*&gt;**
1. Al termine, scegli **OK** e quindi fai clic su **Fine**.

Per creare manualmente un record SRV per un host del Servizio di gestione delle chiavi che usa un server DNS compatibile con BIND 9.x, segui le istruzioni relative a questo tipo di server DNS e specifica le informazioni seguenti per il record SRV:

- Nome:&nbsp; **_vlmcs._TCP**
- Tipo:&nbsp;**SRV**
- Priority: **0**
- Weight: **0**
- Port: **1688**
- Nome host: **&lt;*FQDN o nome dell'host del Servizio di gestione delle chiavi*&gt;**

> [!NOTE]
> Il Servizio di gestione delle chiavi non usa i valori **Priorità** o **Peso**, ma il record deve includerli.

Per configurare un server DNS compatibile con BIND 9.x per supportare la pubblicazione automatica del Servizio di gestione delle chiavi, configura il server DNS in modo da abilitare gli aggiornamenti dei record di risorse dagli host del Servizio di gestione delle chiavi. Ad esempio, aggiungi la riga seguente alla definizione della zona in Named.conf o in Named.conf.local:

```cmd
allow-update { any; };
```
## <a name="manually-assign-a-kms-host-to-a-kms-client"></a>Assegnare manualmente un host del Servizio di gestione delle chiavi a un client del Servizio di gestione delle chiavi

Per impostazione predefinita, i client del Servizio di gestione delle chiavi usano il processo di individuazione automatica. In base a questo processo, un client del Servizio di gestione delle chiavi esegue una query su DNS per cercare un elenco di server che hanno pubblicato record SRV _vlmcs nella zona di appartenenza del client. DNS restituisce l'elenco degli host del Servizio di gestione delle chiavi in ordine casuale. Il client sceglie un host del Servizio di gestione delle chiavi e tenta di stabilire una sessione. Se il tentativo funziona, il client memorizza nella cache il nome dell'host del Servizio di gestione delle chiavi e tenta di usarlo per il tentativo di rinnovo successivo. Se l'impostazione della sessione non riesce, il client sceglie in modo casuale un altro host del Servizio di gestione delle chiavi. È consigliabile usare il processo di individuazione automatica.  

Puoi tuttavia assegnare manualmente un host del Servizio di gestione delle chiavi a un client particolare del Servizio di gestione delle chiavi. Per farlo, eseguire le seguenti operazioni:

1. In un client del Servizio di gestione delle chiavi apri una finestra del prompt dei comandi con privilegi elevati.
1. A seconda dell'implementazione, segui una di queste procedure:
   - Per assegnare un host del Servizio di gestione delle chiavi usando l'FQDN dell'host, esegui il comando seguente:
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
     ```
   - Per assegnare un host del Servizio di gestione delle chiavi usando l'indirizzo IP versione 4 dell'host, esegui il comando seguente:
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <IPv4Address>:<port>
     ```
    - Per assegnare un host del Servizio di gestione delle chiavi usando l'indirizzo IP versione 6 dell'host, esegui il comando seguente:
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <IPv6Address>:<port>
      ```
    - Per assegnare un host del Servizio di gestione delle chiavi usando il nome NETBIOS dell'host, esegui il comando seguente:
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <NETBIOSName>:<port>
      ```
   - Per ripristinare l'individuazione automatica in un client del Servizio di gestione delle chiavi, esegui il comando seguente:
     ```cmd
     cscript \windows\system32\slmgr.vbs -ckms
     ```
     > [!NOTE]
     > Questi comandi usano i segnaposti seguenti:
     >- **<KMS_FQDN>** rappresenta il nome di dominio completo (FQDN) del computer host del Servizio di gestione delle chiavi
     >- **\<IPv4Address\>** rappresenta l'indirizzo IP versione 4 del computer host del Servizio di gestione delle chiavi
     >- **\<IPv6Address\>** rappresenta l'indirizzo IP versione 6 del computer host del Servizio di gestione delle chiavi
     >- **\<NetBiosName\>** rappresenta il nome NETBIOS del computer host del Servizio di gestione delle chiavi
     >- **\<Port\>** rappresenta la porta TCP usata dal Servizio di gestione delle chiavi  

## <a name="configure-the-kms-host-to-publish-in-multiple-dns-domains"></a>Configurare l'host del Servizio di gestione delle chiavi per la pubblicazione in più domini DNS

> [!IMPORTANT]
> Segui con attenzione la procedura descritta in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/help/322756) nel caso in cui si verifichino problemi.

Come descritto in [Assegnare manualmente un host del Servizio di gestione delle chiavi a un client del Servizio di gestione delle chiavi](#manually-assign-a-kms-host-to-a-kms-client), i client del Servizio di gestione delle chiavi usano in genere il processo di individuazione automatica per identificare gli host del Servizio di gestione delle chiavi. Questo processo richiede la disponibilità dei record SRV _vlmcs nella zona DNS del computer client del Servizio di gestione delle chiavi. La zona DNS corrisponde al suffisso DNS primario del computer o a uno degli elementi seguenti:
- Per i computer aggiunti a un dominio, il dominio del computer assegnato dal sistema DNS, ad esempio DNS Active Directory Domain Services.
- Per i computer del gruppo di lavoro, il dominio del computer assegnato dal protocollo DHCP (Dynamic Host Configuration Protocol). Questo nome di dominio è definito dall'opzione con valore di codice 15 secondo quanto definito nella RFC (Request for Comments) 2132.

Per impostazione predefinita, un host del Servizio di gestione delle chiavi registra i record SRV nella zona DNS che corrisponde al dominio del computer host del Servizio di gestione delle chiavi. Supponi ad esempio che un host del Servizio di gestione delle chiavi venga aggiunto al dominio contoso.com. In questo scenario l'host del Servizio di gestione delle chiavi registra il record SRV _vmlcs nella zona DNS contoso.com. Il record identifica quindi il servizio come VLMCS._TCP.CONTOSO.COM.

Se l'host e i client del Servizio di gestione delle chiavi usano zone DNS diverse, dovrai configurare l'host del Servizio di gestione delle chiavi in modo che pubblichi automaticamente i record SRV in più domini DNS. A tale scopo, effettuare le operazioni seguenti:

1. Nell'host del Servizio di gestione delle chiavi avvia l'editor del Registro di sistema. 
1. Individua e quindi seleziona la sottochiave **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL**.
1. Nel riquadro **Dettagli** fai clic con il pulsante destro del mouse in un'area vuota, scegli **Nuovo** e quindi **Valore multistringa**.
1. Per il nome della nuova voce, immetti **DnsDomainPublishList**.
1. Fai clic con il pulsante destro del mouse sulla nuova voce **DnsDomainPublishList** e quindi scegli **Modifica**.
1. Nella finestra di dialogo **Modifica multistringhe** digita ogni suffisso di dominio DNS pubblicato dal Servizio di gestione delle chiavi in una riga separata e quindi scegli **OK**.
   > [!NOTE]
   > Per Windows Server 2008 R2, il formato per **DnsDomainPublishList** è diverso. Per altre informazioni, vedi la Guida di riferimento tecnico per l'attivazione dei contratti multilicenza.
1. Usa lo strumento di amministrazione Servizi per riavviare il Servizio gestione licenze software. Questa operazione crea i record SRV.
1. Verifica che usando un metodo tipico il client del Servizio di gestione delle chiavi possa contattare l'host del Servizio di gestione delle chiavi configurato. Verifica che il client del Servizio di gestione delle chiavi identifichi correttamente l'host del Servizio di gestione delle chiavi per nome e indirizzo IP. Se una di queste verifiche ha esito negativo, esamina questo problema nel resolver del client DNS.
1. Per cancellare i nomi host del Servizio di gestione delle chiavi precedentemente memorizzati nella cache nel client del Servizio di gestione delle chiavi, apri una finestra del prompt dei comandi con privilegi elevati nel client del Servizio di gestione delle chiavi e quindi esegui il comando seguente:
   ```cmd
   cscript C:\Windows\System32\slmgr.vbs -ckms
   ```
