---
title: Principali soluzioni di supporto per Windows Server
description: Ottenere i collegamenti alle soluzioni dei problemi di Windows Server
ms.prod: windows-server
ms.service: na
manager: alant
ms.technology: server-general
ms.date: 03/16/2018
ms.topic: article
author: kaushika-msft
ms.author: elizapo
ms.openlocfilehash: 1ef9d511f8946452c5f9f05628c67c5908c53a55
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402770"
---
# <a name="top-support-solutions-for-windows-server-2016"></a>Principali soluzioni di supporto per Windows Server 2016

Microsoft rilascia regolarmente gli aggiornamenti e le soluzioni per Windows Server. Per garantire che i server ricevano gli aggiornamenti futuri, inclusi gli aggiornamenti della sicurezza, è importante mantenerli aggiornati. Consulta la pagina [Cronologia degli aggiornamenti di Windows 10 e Windows Server 2016](https://support.microsoft.com/en-us/help/4000825/windows-10-windows-server-2016-update-history) per un elenco completo di aggiornamenti rilasciati.

Ecco le principali soluzioni del Supporto tecnico Microsoft per la maggior parte dei problemi comuni rilevati quando si utilizza Windows Server 2016. I collegamenti seguenti includono quelli per articoli KB, aggiornamenti e articoli di libreria.

>[!TIP]
> Cerchi informazioni sull'installazione delle versioni precedenti di Windows Server? vedere le altre [librerie di Windows Server](/previous-versions/windows/) in docs.microsoft.com. Puoi anche [cercare nel sito](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) informazioni specifiche.

## <a name="solutions-for-installing-or-upgrading-windows-server"></a>Soluzioni per l'installazione o l'aggiornamento di Windows Server

- [Risolvere gli errori di aggiornamento di Windows 10: Informazioni tecniche per i professionisti IT](https://docs.microsoft.com/windows/deployment/upgrade/resolve-windows-10-upgrade-errors)
- [Aggiornamento dello stack di manutenzione per Windows 10 versione 1607 e Windows Server 2016: 8 agosto 2017](https://support.microsoft.com/en-US/help/4035631)
- [Aggiornamento della compatibilità per l'aggiornamento a Windows 10 versione 1607 e Windows Server 2016: 3 agosto 2017](https://support.microsoft.com/en-US/help/4033524)
- [Un aggiornamento del sistema sul posto non è supportato nelle macchine virtuali di Azure basate su Windows](https://support.microsoft.com/en-US/help/4014997)
- [Opzioni di aggiornamento e conversione per Windows Server 2016](../get-started/supported-upgrade-paths.md)
- [Matrice di aggiornamento e migrazione dei ruoli server per Windows Server 2016](../get-started/server-role-upgradeability-table.md)
- [Installazione e aggiornamento di Windows Server](../get-started/installation-and-upgrade.md)
- [Note sulla versione: problemi importanti di Windows Server 2016](../get-started/windows-server-2016-ga-release-notes.md)
- [Indicazioni per il passaggio a Windows Server 2016](../get-started/recommendations-moving-to-server2016.md)

## <a name="solutions-for-volume-activation"></a>Soluzioni per l'attivazione di contratti multilicenza
- [Attivazione di Windows Server 2016](../get-started/server-2016-activation.md)
- [Esaminare e selezionare i metodi di attivazione](https://technet.microsoft.com/library/jj134256(ws.11).aspx)
- [Codici di errore di attivazione per l'attivazione dei contratti multilicenza](https://technet.microsoft.com/library/dn502528.aspx)
- [Come risolvere i problemi relativi al servizio di gestione delle chiavi (KMS)](https://technet.microsoft.com/library/ee939272.aspx)
- [Risoluzione dei problemi di attivazione contratti multilicenza](https://technet.microsoft.com/library/ff793439.aspx)
- [Codici di errore di attivazione](https://technet.microsoft.com/library/ff793399.aspx)
- [L'installazione di Windows potrebbe non riuscire con l'errore "il codice Product Key immesso non corrisponde ad alcuna immagine di Windows disponibile per l'installazione. Immettere un codice "Product Key" diverso.](https://support.microsoft.com/help/2796988/windows-8-or-windows-server-2012-installation-may-fail-with-error-mess)

## <a name="solutions-related-to-dcpromo-and-installing-domain-controllers"></a>Soluzioni correlate a DCPromo e installazione di controller di dominio
- [Requisiti delle porte Active Directory e Active Directory Domain Services](https://technet.microsoft.com/library/dd772723(v=ws.10).aspx)
- [Active Directory porte del firewall: si proverà a semplificare questo semplice](http://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/)
- [Supporto di Exchange Server per Windows Server 2016](https://technet.microsoft.com/library/ff728623(v=exchg.150).aspx)
- [Utilizzo di Ntdsutil. exe per trasferire o assegnare ruoli FSMO a un controller di dominio](https://support.microsoft.com/kb/255504)
- [Risoluzione dei problemi relativi alla distribuzione di controller di dominio](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Risoluzione dei problemi di Active Directory installazione guidata](https://msdn.microsoft.com/library/bb727058.aspx)
- [Problemi noti per l'installazione e la rimozione di servizi di dominio Active Directory](https://technet.microsoft.com/library/cc754463(v=ws.10).aspx)

## <a name="solutions-for-active-directory-federation-services-ad-fs"></a>Soluzioni per Active Directory Federation Services (AD FS)
- [Come configurare la registrazione automatica dei dispositivi Windows aggiunti a un dominio con Azure Active Directory](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)
- [Rilascio di attestazioni di installazione](/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup#step-2-setup-issuance-of-claims)
- [Configurare AD FS per l'autenticazione di utenti memorizzati nelle directory LDAP](../identity/ad-fs/operations/configure-ad-fs-to-authenticate-users-stored-in-ldap-directories.md)
- [Supporto di AD FS per l'associazione di nomi host alternativi per l'autenticazione del certificato](../identity/ad-fs/operations/ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Proteggi dagli attacchi con password](https://blogs.technet.microsoft.com/tspring/2017/01/20/federated-to-microsoft-cloud-and-account-lockouts/)
- [Aggiornamento ad AD FS in Windows Server 2016 con un database interno di Windows](../identity/ad-fs/deployment/upgrading-to-ad-fs-in-windows-server-2016.md)
- [Accesso a Windows 10-Abilitazione dell'autenticazione del dispositivo con AD FS](../identity/ad-fs/operations/configure-device-based-conditional-access-on-premises.md)
- [Gestione dei certificati SSL in AD FS e WAP in Windows Server 2016](../identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap-2016.md)
- [Criteri di controllo degli accessi in Windows Server 2016 AD FS](../identity/ad-fs/operations/access-control-policies-in-ad-fs.md)

## <a name="solutions-related-to-active-directory-replication"></a>Soluzioni correlate alla replica di Active Directory

- [Risoluzione dei problemi di replica di Active Directory](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [Scaricare Stato replica di Active Directory strumento dall'area download Microsoft](https://www.microsoft.com/en-in/download/details.aspx?id=30005)
- [E2E Risoluzione degli errori comuni di replica di Active Directory](https://support.microsoft.com/kb/3108513)
- [Risoluzione degli errori di replica di Active Directory 8606: Attributi insufficienti per la creazione di un oggetto](https://support.microsoft.com/kb/2028495)
- [L'ID evento 2108 e l'ID evento 1084 si verificano durante la replica in ingresso di Active Directory in Windows 2000 Server e in Windows Server 2003](https://support.microsoft.com/kb/837932)
- [Risoluzione degli errori di replica di Active Directory 8451: Si è verificato un errore di database durante l'operazione di replica](https://support.microsoft.com/kb/2645996)
- [Risoluzione degli errori di replica di Active Directory 1127: Durante l'accesso al disco rigido, un'operazione su disco non è riuscita anche dopo i tentativi](https://support.microsoft.com/kb/2025726)
- [Pulisci metadati del server](https://technet.microsoft.com/library/cc816907.aspx)