---
title: rpcping
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7382aa0d-90fc-47c0-84b3-15f52dd656d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfa1d08c81f8b26507a5cae5f688923a7b226e1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829992"
---
# <a name="rpcping"></a>rpcping

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conferma la connettività RPC tra il computer che eseguono Microsoft Exchange Server e una delle workstation Client di Microsoft Exchange supportate nella rete. Questa utilità può essere utilizzata per verificare se i servizi di Microsoft Exchange Server rispondono alle richieste RPC dalla propria workstation client tramite la rete. 

## <a name="syntax"></a>Sintassi
```
rpcping [/t <protseq>] [/s <server_addr>] [/e <endpoint>
        |/f <interface UUID>[,Majorver]] [/O <Interface Object UUID]
        [/i <#_iterations>] [/u <security_package_id>] [/a <authn_level>]
        [/N <server_princ_name>] [/I <auth_identity>] [/C <capabilities>]
        [/T <identity_tracking>] [/M <impersonation_type>]
        [/S <server_sid>] [/P <proxy_auth_identity>] [/F <RPCHTTP_flags>]
        [/H <RPC/HTTP_authn_schemes>] [/o <binding_options>]
        [/B <server_certificate_subject>] [/b] [/E] [/q] [/c]
        [/A <http_proxy_auth_identity>] [/U <HTTP_proxy_authn_schemes>]
        [/r <report_results_interval>] [/v <verbose_level>] [/d]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|/t \<protseq>|Specifica la sequenza di protocollo da usare. Può essere uno delle sequenze del protocollo RPC standard, ad esempio: ncacn_ip_tcp, ncacn_np, o ncacn_http.<br /><br />Se non specificato, il valore predefinito è ncacn_ip_tcp.|
|/s \<server_addr>|Specifica l'indirizzo del server. Se non specificato, che verrà inviato un ping nel computer locale.|
|/e \<endpoint>|Specifica l'endpoint per eseguire il ping. Se non viene specificato, verrà eseguito il ping di mapping degli endpoint nel computer di destinazione.<br /><br />Questa opzione si escludono a vicenda con l'interfaccia (**/f**) opzione.|
|/o \<binding_options>|Specifica le opzioni di associazione per il ping RPC.|
|/f \<UUID di interfaccia > [, Majorver]|Specifica l'interfaccia per effettuare il ping. Questa opzione si escludono a vicenda con l'opzione di endpoint. L'interfaccia viene specificato come un UUID.<br /><br />Se il *Majorver* non viene specificato, verrà cercata la versione 1 dell'interfaccia.<br /><br />Quando si specifica interfaccia, **rpcping** eseguirà una query il mapping degli endpoint nel computer di destinazione per recuperare l'endpoint per l'interfaccia specificata. Mapping degli endpoint verranno sottoposti a query usando le opzioni specificate nella riga di comando.|
|/O \<oggetto UUID >|Specifica l'oggetto UUID se l'interfaccia registrata uno.|
|/i \<#_iterations>|Specifica il numero di chiamate per rendere. Il valore predefinito è 1. Questa opzione è utile per misurare la latenza della connessione se vengono specificate più iterazioni.|
|/u \<security_package_id>|Specifica il pacchetto di sicurezza (provider di sicurezza) per effettuare la chiamata deve usare RPC. Il pacchetto di sicurezza è identificato come un numero o un nome. Se viene utilizzato un numero è lo stesso numero di API RpcBindingSetAuthInfoEx. L'elenco seguente mostra i nomi e numeri. I nomi non sono tra maiuscole e minuscole:<br /><br />-Negotiate / 9 o uno dei nego, snego o negoziare<br />-NTLM / 10 o NTLM<br />-SChannel / 14 o SChannel<br />-Kerberos / 16 o Kerberos<br />-Kernel / 20 o del Kernel<br />    Se si specifica questa opzione, è necessario specificare il livello di autenticazione diverso da none. Non vi è alcun valore predefinito per questa opzione. Se non è specificato, RPC non userà sicurezza per il ping.|
|/a \<authn_level>|Specifica il livello di autenticazione da usare. I valori possibili sono:<br /><br />-connect<br />-chiamata<br />-pkt<br />-l'integrità<br />-   privacy<br /><br />Se questa opzione viene specificata, la sicurezza ID di pacchetto (/ u) deve essere specificato anche. Non vi è alcun valore predefinito per questa opzione.<br /><br />Se questa opzione non è specificata, RPC non userà sicurezza per il ping.|
|/N \<server_princ_name >|Specifica il nome dell'entità server.<br /><br />Questo campo può essere usato solo quando viene selezionato il pacchetto di sicurezza e a livello di autenticazione.|
|/I \<auth_identity>|Consente di specificare le identità alternative per la connessione al server. L'identità è nel formato utente, dominio, la password. Se il nome utente, dominio o la password è caratteri speciali che possono essere interpretati dalla shell, racchiudere l'identità tra virgolette doppie. È possibile specificare **\*** invece della password, verrà richiesto di immettere la password senza visualizzarla sullo schermo. Se questo campo non è specificato, verrà utilizzato l'identità dell'utente connesso.<br /><br />Questo campo può essere usato solo quando viene selezionato il pacchetto di sicurezza e a livello di autenticazione.|
|/C \<capabilities>|Specifica una maschera di bit di flag esadecimale. Questo campo può essere usato solo quando viene selezionato il pacchetto di sicurezza e a livello di autenticazione.|
|/T \<identity_tracking>|Specifica statica o dinamica. Se non specificato, dinamica è l'impostazione predefinita.<br /><br />Questo campo può essere usato solo quando viene selezionato il pacchetto di sicurezza e a livello di autenticazione.|
|/M \<impersonation_type>|Specifica anonima, identificare, rappresentano o delegano. Rappresentare l'impostazione predefinita.<br /><br />Questo campo può essere usato solo quando viene selezionato il pacchetto di sicurezza e a livello di autenticazione.|
|/S \<server_sid>|Specifica il SID del server previsto.<br /><br />Questo campo può essere usato solo quando viene selezionato il pacchetto di sicurezza e a livello di autenticazione.|
|/P \<proxy_auth_identity>|Specifica l'identità per l'autenticazione con il proxy RPC/HTTP. Ha lo stesso formato per l'opzione /i. È necessario specificare il pacchetto di sicurezza (/ u), livello di autenticazione (/a) e gli schemi di autenticazione (/ H) per poter usare questa opzione.|
|/F \<RPCHTTP_flags>|Specifica i flag da passare per l'autenticazione del front-end RPC/HTTP. Il flag può essere specificato come numeri o nomi che i flag attualmente riconosciuti sono:<br /><br />-Usare SSL / 1 o ssl o use_ssl<br />-Usa primo schema di autenticazione / 2 oppure prima o nell'use_first<br /><br />È necessario specificare il pacchetto di sicurezza (/ u) e livello di autenticazione (/) per poter usare questa opzione.|
|/H \<RPC/HTTP_authn_schemes>|Specifica gli schemi di autenticazione da usare per l'autenticazione del front-end RPC/HTTP. Questa opzione è un elenco di valori numerici o di nomi separati da virgole. Esempio: Basic, NTLM. I valori riconosciuti sono (nomi non fanno distinzione maiuscole / minuscole):<br /><br />-Base / 1 o Basic<br />-NTLM / 2 o NTLM<br />-Certificati / 65536 o certificato<br /><br />È necessario specificare il pacchetto di sicurezza (/ u) e livello di autenticazione (/) per poter usare questa opzione.|
|/B \<server_certificate_subject>|Specifica il soggetto del certificato server. È necessario usare SSL per questa opzione per lavorare.<br /><br />È necessario specificare il pacchetto di sicurezza (/ u) e livello di autenticazione (/) per poter usare questa opzione.|
|/ b|Recupera l'oggetto del certificato server dal certificato inviato dal server e visualizza una schermata o un file di log. Valido solo quando il Proxy echo solo opzione (/ E) e vengono specificate le opzioni SSL usare.<br /><br />È necessario specificare il pacchetto di sicurezza (/ u) e livello di autenticazione (/) per poter usare questa opzione.|
|/R|Specifica il proxy HTTP. Se *none*, viene utilizzato il proxy RPC. Il valore *predefinito* significa che usare le impostazioni di Internet Explorer nel computer client. Qualsiasi altro valore viene considerato come il proxy HTTP esplicito. Se non si specifica questo flag, si presuppone il valore predefinito, vale a dire, vengono controllate le impostazioni di Internet Explorer. Questo flag è valido solo quando la **/E** flag (solo echo) è abilitato.|
|/E|Limita il ping al solo il proxy RPC/HTTP. Il ping non raggiungerà il server. È utile quando si tenta di stabilire se il proxy RPC/HTTP è raggiungibile. Per specificare un proxy HTTP, usare il flag /R. Se viene specificato un proxy HTTP nel flag di /o, questa opzione verrà ignorata.<br /><br />È necessario specificare il pacchetto di sicurezza (/ u) e livello di autenticazione (/) per poter usare questa opzione.|
|/q|Specifica la modalità non interattiva. Non inviare alcuna richiesta, ad eccezione delle password. Si presuppone *Y* risposta a tutte le query. Usare questa opzione con cautela.|
|/c|Usare il certificato della smart card. RPCPing richiederà all'utente di scegliere delle smart card.|
|/A|Specifica l'identità con cui eseguire l'autenticazione a proxy HTTP. Ha lo stesso formato per l'opzione /i.<br /><br />È necessario specificare gli schemi di autenticazione (/ U), pacchetto di sicurezza (/ u) e livello di autenticazione (/) per poter usare questa opzione.|
|/U|Specifica gli schemi di autenticazione da utilizzare per l'autenticazione del proxy HTTP. Questa opzione è un elenco di valori numerici o di nomi separati da virgole. Esempio: Basic, NTLM. I valori riconosciuti sono (nomi non fanno distinzione maiuscole / minuscole):<br /><br />-Base / 1 o Basic<br />-NTLM / 2 o NTLM<br /><br />È necessario specificare il pacchetto di sicurezza (/ u) e livello di autenticazione (/) per poter usare questa opzione.|
|/r|Se vengono specificate più iterazioni, questa opzione renderà **rpcping** visualizzare le statistiche di esecuzione correnti periodicamente invece dopo l'ultima chiamata. L'intervallo di report è espresso in secondi. Valore predefinito è 15.|
|/v|Indica a **rpcping** il livello di dettaglio per rendere l'output. Valore predefinito è 1. 2 e 3 specificare restituito ulteriori **rpcping**.|
|/d|Avvia RPC diagnostica dell'interfaccia utente di rete.|
|/ p|Specifica per richiedere le credenziali se l'autenticazione ha esito negativo.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi
Per scoprire se il server di Exchange che connessione tramite RPC/HTTP sia accessibile, digitare:
```
rpcping /t ncacn_http /s exchange_server /o RpcProxy=front_end_proxy /P "username,domain,*" /H Basic /u NTLM /a connect /F 3
```

## <a name="additional-references"></a>Altri riferimenti
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
