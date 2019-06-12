---
title: Principali soluzioni di supporto per Windows Server
description: Ottenere i collegamenti alle soluzioni dei problemi di Windows Server
ms.prod: windows-server-threshold
ms.service: na
manager: alant
ms.technology: server-general
ms.date: 03/16/2018
ms.topic: article
author: kaushika-msft
ms.author: elizapo
ms.openlocfilehash: 1eb52f28fcd5afe62df33cd56208f2ab506e7194
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453155"
---
# <a name="top-support-solutions-for-windows-server-2016"></a>Principali soluzioni di supporto per Windows Server 2016

Microsoft rilascia regolarmente gli aggiornamenti e le soluzioni per Windows Server. Per garantire che i server ricevano gli aggiornamenti futuri, inclusi gli aggiornamenti della sicurezza, è importante mantenerli aggiornati. Consulta la pagina [Cronologia degli aggiornamenti di Windows 10 e Windows Server 2016](https://support.microsoft.com/en-us/help/4000825/windows-10-windows-server-2016-update-history) per un elenco completo di aggiornamenti rilasciati.

Ecco le principali soluzioni del Supporto tecnico Microsoft per la maggior parte dei problemi comuni rilevati quando si utilizza Windows Server 2016. I collegamenti seguenti includono quelli per articoli KB, aggiornamenti e articoli di libreria.

>[!TIP]
> Cerchi informazioni sull'installazione delle versioni precedenti di Windows Server? vedere le altre [librerie di Windows Server](/previous-versions/windows/) in docs.microsoft.com. Puoi anche [cercare nel sito](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) informazioni specifiche.

## <a name="solutions-for-installing-or-upgrading-windows-server"></a>Soluzioni per l'installazione o l'aggiornamento di Windows Server

- [Risolvere gli errori di aggiornamento di Windows 10: Informazioni tecniche per i professionisti IT](https://docs.microsoft.com/windows/deployment/upgrade/resolve-windows-10-upgrade-errors)
- [Aggiornamento dello stack di manutenzione di Windows 10 versione 1607 e Windows Server 2016: 8 agosto 2017](https://support.microsoft.com/en-US/help/4035631)
- [Aggiornamento di compatibilità per l'aggiornamento a Windows 10 versione 1607 e Windows Server 2016: 3 agosto 2017](https://support.microsoft.com/en-US/help/4033524)
- [Un aggiornamento sul posto del sistema non è supportato nelle macchine virtuali di Azure basata su Windows](https://support.microsoft.com/en-US/help/4014997)
- [Opzioni di aggiornamento e conversione per Windows Server 2016](../get-started/supported-upgrade-paths.md)
- [Matrice server ruolo aggiornamento e migrazione per Windows Server 2016](../get-started/server-role-upgradeability-table.md)
- [Aggiornamento e installazione di Windows Server](../get-started/installation-and-upgrade.md)
- [Note sulla versione: problemi importanti di Windows Server 2016](../get-started/windows-server-2016-ga-release-notes.md)
- [Indicazioni per il passaggio a Windows Server 2016](../get-started/recommendations-moving-to-server2016.md)

## <a name="solutions-for-volume-activation"></a>Soluzioni per l'attivazione di contratti multilicenza
- [Windows Server 2016 Activation](../get-started/server-2016-activation.md)
- [Revisione e i metodi di attivazione selezionare](https://technet.microsoft.com/library/jj134256(ws.11).aspx)
- [Codici di errore di attivazione contratti multilicenza](https://technet.microsoft.com/library/dn502528.aspx)
- [Come risolvere il servizio di gestione di chiavi (KMS)](https://technet.microsoft.com/library/ee939272.aspx)
- [Risoluzione dei problemi di Volume Activation](https://technet.microsoft.com/library/ff793439.aspx)
- [Codici di errore di attivazione](https://technet.microsoft.com/library/ff793399.aspx)
- [Installazione di Windows potrebbe non riuscire con l'errore "il codice product key immesso non corrisponde ad alcuna delle immagini Windows disponibili per l'installazione. Immettere un diverso codice product key"](https://support.microsoft.com/help/2796988/windows-8-or-windows-server-2012-installation-may-fail-with-error-mess)

## <a name="solutions-related-to-dcpromo-and-installing-domain-controllers"></a>Soluzioni correlate a DCPromo e installazione di controller di dominio
- [Active Directory e i requisiti di porta di Active Directory Domain Services](https://technet.microsoft.com/library/dd772723(v=ws.10).aspx)
- [Porte del Firewall per Active Directory: è possibile provare a rendere questo semplice](http://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/)
- [Supporto di Exchange Server per Windows Server 2016](https://technet.microsoft.com/library/ff728623(v=exchg.150).aspx)
- [Uso di Ntdsutil.exe per assegnare o trasferire ruoli FSMO a un controller di dominio](https://support.microsoft.com/kb/255504)
- [Risoluzione dei problemi relativi alla distribuzione di controller di dominio](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Risoluzione dei problemi di installazione guidata di Active Directory](https://msdn.microsoft.com/library/bb727058.aspx)
- [Problemi noti per l'installazione e rimozione di Active Directory Domain Services](https://technet.microsoft.com/library/cc754463(v=ws.10).aspx)

## <a name="solutions-for-active-directory-federation-services-ad-fs"></a>Soluzioni per Active Directory Federation Services (AD FS)
- [Come configurare la registrazione automatica dei dispositivi aggiunti a un dominio di Windows con Azure Active Directory](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)
- [Configurare il rilascio delle attestazioni](/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup#step-2-setup-issuance-of-claims)
- [Configurare AD FS per l'autenticazione di utenti memorizzati nelle directory LDAP](../identity/ad-fs/operations/configure-ad-fs-to-authenticate-users-stored-in-ldap-directories.md)
- [Supporto di AD FS per l'associazione di nomi host alternativi per l'autenticazione del certificato](../identity/ad-fs/operations/ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Protezione contro gli attacchi alle password](https://blogs.technet.microsoft.com/tspring/2017/01/20/federated-to-microsoft-cloud-and-account-lockouts/)
- [Aggiornamento ad AD FS in Windows Server 2016 con un database interno di Windows](../identity/ad-fs/deployment/upgrading-to-ad-fs-in-windows-server-2016.md)
- [Firma di Windows 10 su – come abilitare l'autenticazione dei dispositivi con AD FS](../identity/ad-fs/operations/configure-device-based-conditional-access-on-premises.md)
- [La gestione dei certificati SSL in AD FS e WAP in Windows Server 2016](../identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap-2016.md)
- [Criteri di controllo di accesso in Windows Server 2016 AD ADFS](../identity/ad-fs/operations/access-control-policies-in-ad-fs.md)

## <a name="solutions-related-to-active-directory-replication"></a>Soluzioni correlate alla replica di Active Directory

- [Risoluzione dei problemi di replica di Active Directory](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [Scaricare lo strumento di Active Directory replica lo stato dall'area download Microsoft](https://www.microsoft.com/en-in/download/details.aspx?id=30005)
- [E2E: Come risolvere i problemi più comuni errori di replica di Active Directory](https://support.microsoft.com/kb/3108513)
- [Risoluzione dell'errore di replica di Active Directory 8606: Sono stati specificati attributi insufficienti per creare un oggetto](https://support.microsoft.com/kb/2028495)
- [ID evento 2108 e 1084 ID eventi si verificano durante la replica in ingresso di Active Directory in Windows 2000 Server e in Windows Server 2003](https://support.microsoft.com/kb/837932)
- [Risoluzione dei problemi 8451 errore di replica di Active Directory: L'operazione di replica si è verificato un errore di database](https://support.microsoft.com/kb/2645996)
- [Errore di replica di Active Directory 1127 sulla risoluzione dei problemi: Durante l'accesso al disco rigido, un'operazione del disco non è riuscita anche dopo vari tentativi](https://support.microsoft.com/kb/2025726)
- [La pulizia dei metadati del Server](https://technet.microsoft.com/library/cc816907.aspx)