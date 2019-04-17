---
title: "Installare Windows Server Essentials in modalità di migrazione 1"
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7196ac-cfa6-46a5-ba77-6962b47a825e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 808a4b1e120fa559d603b34ad006b18de6b94378
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-windows-server-essentials-in-migration-mode1"></a>Installare Windows Server Essentials in modalità di migrazione 1

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile avere un solo server sulla rete che esegue Windows Server Essentials e tale server deve essere un controller di dominio per la rete.  
  
 Quando si installa Windows Server Essentials in modalità di migrazione, l'installazione guidata esegue le attività seguenti:  
  
1.  Installa e configura il software server di Windows Server Essentials nel Server di destinazione.  
  
2.  Aggiorna lo schema del dominio per la versione più recente.  
  
3.  Aggiunge il Server di destinazione al dominio esistente. Il Server di origine e il Server di destinazione possono essere entrambi membri dello stesso dominio finché non viene completato il processo di migrazione. Al termine della migrazione, è necessario rimuovere il Server di origine dalla rete entro 21 giorni.  
  
    > [!WARNING]
    >  Un messaggio di errore viene aggiunto al registro eventi ogni giorno durante il periodo di prova di 21 giorni finché non si rimuove il Server di origine dalla rete. Il testo del messaggio, "controllo del ruolo FSMO: rilevato ambiente non conforme al criterio di gestione licenze. Il Server di gestione deve contenere il controller di dominio primario e il dominio master nomi Active Directory. Spostare i ruoli di Active Directory per il Server di gestione. Il server verrà automaticamente arrestato se il problema non viene risolto entro 21 giorni dal momento in che cui è stata rilevata questa condizione." Dopo il periodo di tolleranza di 21 giorni, il Server di origine verrà arrestato.  
  
4.  Trasferisce i ruoli di master (detto anche FSMO, flexible single master Operation) operazioni dal Server di origine al Server di destinazione. Ruoli di master operazioni sono attività del controller di dominio specifico che vengono utilizzate quando i metodi standard di trasferimento dei dati e aggiornamento si rivelano inadeguati. Quando il Server di destinazione diventa un controller di dominio, deve contenere le operazioni di ruoli di master.  
  
5.  Configura il Server di destinazione come server di catalogo globale. Il server di catalogo globale è un controller di dominio che gestisce un archivio dati distribuiti. Contiene una rappresentazione parziale e ricercabile di ogni oggetto in ogni dominio della foresta di Active Directory.  
  
6.  Configura il Server di destinazione come server di gestione licenze.  
  
##  <a name="BKMK_Install"></a>Installare Windows Server Essentials nel Server di destinazione  
 Per installare e configurare Windows Server Essentials nel Server di destinazione in modalità di migrazione, eseguire la procedura seguente.  
  
#### <a name="to-install-windows-server-essentials-on-the-destination-server"></a>Per installare Windows Server Essentials nel Server di destinazione  
  
1.  Attivare il Server di destinazione e inserire l'unità DVD DVD1 di Windows Server Essentials. Se viene visualizzato un messaggio che chiede se si desidera eseguire l'avvio da un CD o DVD, premere un tasto qualsiasi per eseguire questa operazione.  
  
    > [!NOTE]
    >  Se il Server di destinazione supporta l'avvio da un'unità flash USB, è possibile utilizzare il **strumento di Download USB/DVD di Windows 7** per creare un'unità Flash USB avviabile dal file ISO di Windows Server Essentials. Con un'unità flash USB può velocizzare notevolmente il processo di installazione perché le unità flash leggono i dati molto più rapidamente delle unità DVD-ROM. Dopo aver creato un'unità flash USB avviabile, è possibile aggiungere un file di risposte nell'unità flash. È possibile [scaricare lo strumento di Download USB/DVD di Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=248282) gratuitamente dal sito Web Microsoft Store.  
  
    > [!NOTE]
    >  Se il Server di destinazione non viene avviato dal DVD, riavviare il computer e controllare la configurazione del BIOS per assicurarsi che **DVD-ROM** è elencato per primo nella sequenza di avvio. Per ulteriori informazioni su come modificare la sequenza di avvio di configurazione del BIOS, vedere la documentazione del produttore dell'hardware.  
  
2.  Fare clic su **nuova installazione**.  
  
3.  Se si dispone di un disco rigido interno non viene visualizzato nell'elenco, fare clic su **Carica driver** e installare il driver necessario prima di continuare.  
  
4.  Selezionare la casella di controllo che consente di verificare tutti i file e cartelle sul disco rigido primaria verranno eliminate e quindi fare clic su **installare**.  
  
5.  Nel **Scegli modalità di installazione server** pagina, fare clic su **migrazione del Server**, quindi fornire le informazioni sulla migrazione richieste.  
  
6.  Quando il **correttamente la migrazione del server** messaggio viene visualizzato, fare clic su **Chiudi**.  
  
 Al termine dell'installazione, accede automaticamente con l'account dell'utente amministratore e la password specificati nel file di risposte della migrazione.  
  
> [!NOTE]
>  Per sbloccare il desktop durante l'installazione di Windows Server Essentials, usare l'account predefinito administrator e lasciare la password vuota.  
  
##  <a name="BKMK_VerifyTheHealthOfDC"></a>Verificare l'integrità del controller di dominio  
 Prima di procedere con la migrazione, è necessario assicurarsi che il controller di dominio e di rete Windows Server Essentials siano integri.  
  
 Nella tabella seguente sono elencati gli strumenti che è possibile utilizzare per diagnosticare eventuali problemi nel Server di destinazione e della rete e nel dominio:  
  
|Strumento|Descrizione|  
|----------|-----------------|  
|Netdiag|Consente di isolare problemi di connettività e di rete. Per ulteriori informazioni e per il download, vedere [Netdiag](https://go.microsoft.com/fwlink/?LinkId=217388).|  
|Dcdiag.exe|Analizza lo stato dei controller di dominio in una foresta o dell'organizzazione e segnala i problemi per facilitare la risoluzione dei problemi. Per ulteriori informazioni e per il download, vedere [Dcdiag](https://go.microsoft.com/fwlink/?LinkId=217389).|  
|Repadmin.exe|Consente di diagnosticare i problemi di replica tra controller di dominio. Questo strumento richiede l'esecuzione dei parametri della riga di comando. Per ulteriori informazioni e per il download, vedere [Repadmin](https://go.microsoft.com/fwlink/?LinkId=217387).|  
  
 È necessario correggere tutti i problemi che questi report strumenti prima di procedere con la migrazione.  
  
> [!NOTE]
>  Se si prevede di eseguire la migrazione di posta elettronica a un altro server di Exchange in locale, vedere [integrare un Server Exchange locale con Windows Server Essentials](../manage/Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md) per informazioni su come configurare il server Exchange locale.
