---
title: I client non riescono a connettersi e ricevono l'errore Nessuna licenza disponibile
description: Risoluzione dei problemi relativi alla mancata disponibilità di licenze con una connessione Desktop remoto
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 74b4656216d5568f546d1a13722eeec748f1f9b5
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265893"
---
# <a name="clients-cant-connect-and-see-no-licenses-available-error"></a>I client non riescono a connettersi e ricevono l'errore "Nessuna licenza disponibile"

Questa situazione si verifica per le distribuzioni che includono un server Host sessione Desktop remoto e un server licenze Desktop remoto.

Prima di tutto, identifica il comportamento rilevato dagli utenti:

- La sessione è stata disconnessa perché non sono disponibili licenze o server licenze.
- L'accesso è stato negato a causa di un errore di sicurezza.

Accedi all'host sessione Desktop remoto come amministratore di dominio e apri lo strumento Diagnosi Servizio licenze Desktop remoto. Cerca messaggi simili ai seguenti:

  - Il periodo di prova per il server Host sessione Desktop remoto è scaduto, ma tale server non è stato configurato con alcun server licenze. Le connessioni al server Host sessione Desktop remoto verranno negate a meno che non sia configurato un server licenze per tale server.
  - Server licenze \<nome computer\> non disponibile. Ciò può verificarsi a causa di problemi relativi alla connettività di rete, all'arresto del Servizio licenze Desktop remoto nel server licenze oppure alla mancata disponibilità del Servizio licenze Desktop remoto.

Questi problemi tendono a essere associati ai messaggi utente seguenti:

  - La sessione remota è stata disconnessa perché non sono disponibili licenze CAL (Client Access License) di Desktop remoto per il PC.
  - La sessione remota è stata disconnessa perché non sono disponibili server licenze di Desktop remoto per il rilascio della licenza.

In questo caso, [configura il servizio licenze Desktop remoto](#configure-the-rd-licensing-service).

Se lo strumento Diagnosi Servizio licenze Desktop remoto segnala altri problemi, ad esempio "Il componente del protocollo RDP X.224 ha rilevato un errore nel flusso del protocollo e ha disconnesso il client", può essere presente un problema che incide sui certificati di licenza. Tali problemi tendono a essere associati a messaggi utente simili al seguente:

Impossibile connettersi al server terminal. Errore di sicurezza. Dopo aver verificato di essere connessi alla rete, provare a riconnettersi al server.

In questo caso, [aggiorna le chiavi del Registro di sistema per il certificato X509](#refresh-the-x509-certificate-registry-keys).

## <a name="configure-the-rd-licensing-service"></a>Configurare il servizio licenze Desktop remoto

Nella procedura seguente viene usato Server Manager per apportare le modifiche relative alla configurazione. Per informazioni su come configurare e usare Server Manager, vedi [Server Manager](../../../administration/server-manager/server-manager.md)

1. Apri **Server Manager** e passa a **Servizi Desktop remoto**.
2. In **Panoramica della distribuzione** seleziona **Attività** e quindi **Modifica proprietà distribuzione**.
3. Seleziona **Servizio licenze Desktop remoto** e quindi scegli la modalità gestione licenze appropriata per la distribuzione (**Per Dispositivo** o **Per Utente**).
4. Immetti il nome di dominio completo del server licenze Desktop remoto e quindi fai clic su **Aggiungi**.
5. Se disponi di più server licenze Desktop remoto, ripeti il passaggio 4 per ogni server. 
    ![Opzioni di configurazione del server licenze Desktop remoto in Server Manager.](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

## <a name="refresh-the-x509-certificate-registry-keys"></a>Aggiornare le chiavi del Registro di sistema per il certificato X509

> [!IMPORTANT]  
> Segui attentamente le istruzioni della sezione. Un'errata modifica del Registro di sistema può causare gravi problemi. Prima di iniziare a modificare il Registro di sistema, [eseguine il backup](https://support.microsoft.com/help/322756) in modo da poterlo ripristinare in caso di problemi.

Per risolvere questo problema, esegui il backup e quindi rimuovi le chiavi del Registro di sistema relative al certificato X509, riavvia il computer e infine riattiva il server licenze Desktop remoto. Segui questa procedura.

> [!NOTE]
> Esegui la procedura seguente in ognuno dei server Host sessione Desktop remoto.

Ecco come riattivare il server licenze Desktop remoto:

1. Apri l'editor del Registro di sistema e passa a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. Scegli **Esporta file del Registro di sistema** dal menu del Registro di sistema.
3. Immetti **exported- Certificate** nella casella **Nome file** e quindi seleziona **Salva**.
4. Fai clic con il pulsante destro del mouse su ognuno dei valori seguenti, scegli **Elimina** e quindi fai clic su **Sì** per confermare l'eliminazione:  
      - **Certificato**
      - **Certificato X509**
      - **ID certificato X509**
      - **X509 Certificate2**
5. Esci dall'editor del Registro di sistema e riavvia il server Host sessione Desktop remoto.