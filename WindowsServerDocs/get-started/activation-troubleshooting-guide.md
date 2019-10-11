---
title: Risoluzione dei problemi di attivazione dei contratti multilicenza per Windows
description: Questo articolo elenca le risorse che includono informazioni sulle procedure consigliate per l'attivazione dei contratti multilicenza e sulla risoluzione dei problemi di attivazione
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: 09206b90dc8b829aaa70d0cca34bd05a9e0eb693
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963047"
---
# <a name="troubleshooting-windows-volume-activation"></a>Risoluzione dei problemi di attivazione dei contratti multilicenza per Windows

L'attivazione di un prodotto è il processo di convalida del software dopo l'installazione in un computer specifico. L'attivazione conferma che il prodotto è originale, e non una copia fraudolenta, e che il codice Product Key o il numero di serie è valido e non è stato compromesso o revocato. Con l'attivazione viene anche stabilito un collegamento, o relazione, tra il codice Product Key e l'installazione.

L'attivazione dei contratti multilicenza è il processo di attivazione dei prodotti con contratti multilicenza. Per usare un contratto multilicenza, l'organizzazione deve stipulare un apposito contratto con Microsoft. Microsoft offre programmi personalizzati per contratti multilicenza in base alle dimensioni e alle preferenze di acquisto delle organizzazioni. Per altre informazioni, visita [Microsoft Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).

La [Guida di attivazione di Windows Server 2016](server-2016-activation.md) è incentrata sulla tecnologia di attivazione basata sul Servizio di gestione delle chiavi (KMS, Key Management Service). In questa sezione vengono descritti i problemi comuni e vengono indicate linee guida per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi e ad altre tecnologie di attivazione dei contratti multilicenza.

## <a name="best-practices-for-volume-activation"></a>Procedure consigliate per l'attivazione dei contratti multilicenza

Gli articoli seguenti includono informazioni tecniche e procedure consigliate per le tecnologie di attivazione dei contratti multilicenza di Microsoft.

### <a name="key-management-service-kms"></a>Servizio di gestione delle chiavi

- [Pianificare l'attivazione dei contratti multilicenza](https://docs.microsoft.com/windows/deployment/volume-activation/plan-for-volume-activation-client)
- [Informazioni sul Servizio di gestione delle chiavi](https://docs.microsoft.com/previous-versions/tn-archive/ff793434(v=technet.10))
- [Distribuzione dell'attivazione tramite il Servizio di gestione delle chiavi](https://docs.microsoft.com/previous-versions/tn-archive/ff793409%28v=technet.10%29)
- [Configurazione degli host del Servizio di gestione delle chiavi](https://docs.microsoft.com/previous-versions/tn-archive/ff793407%28v%3dtechnet.10%29)
- [Configurazione del DNS](https://docs.microsoft.com/previous-versions/tn-archive/ff793405%28v%3dtechnet.10%29)
- [Attivare tramite il Servizio di gestione delle chiavi](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-key-management-service-vamt)

### <a name="active-directory-based-activation-adba"></a>Attivazione basata su Active Directory

- [Distribuire l'attivazione basata su Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502534%28v%3Dws.11%29)
- [Attivare tramite l'attivazione basata su Active Directory](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-active-directory-based-activation-client)
- [Panoramica dell'attivazione basata su Active Directory](https://docs.microsoft.com/windows/deployment/volume-activation/active-directory-based-activation-overview)

### <a name="multiple-activation-key-mak-activation"></a>Attivazione tramite codice ad attivazione multipla (MAK)

- [Uso dell'attivazione tramite codice ad attivazione multipla](https://docs.microsoft.com/previous-versions/tn-archive/ff793438%28v=technet.10%29)
- [Informazioni sull'attivazione tramite codice ad attivazione multipla](https://docs.microsoft.com/previous-versions/tn-archive/ff793435%28v%3dtechnet.10%29)
- [Attivazione dei client MAK](https://docs.microsoft.com/previous-versions/tn-archive/ff793398%28v%3dtechnet.10%29)

### <a name="subscription-activation"></a>Attivazione dell'abbonamento

- [Attivazione dell'abbonamento per Windows 10](https://docs.microsoft.com/windows/deployment/windows-10-subscription-activation)
- [Distribuire le licenze di Windows 10 Enterprise](https://docs.microsoft.com/windows/deployment/deploy-enterprise-licenses)
- [Windows 10 Enterprise E3 in CSP](https://docs.microsoft.com/windows/deployment/windows-10-enterprise-e3-overview)

## <a name="resources-for-troubleshooting-activation-issues"></a>Risorse per la risoluzione dei problemi di attivazione

Gli articoli seguenti includono linee guida e informazioni sugli strumenti per la risoluzione dei problemi di attivazione dei contratti multilicenza:

- [Linee guida per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi (KMS)](activation-troubleshoot-kms-general.md)
- [Opzioni di Slmgr.vbs per ottenere informazioni sull'attivazione dei contratti multilicenza](activation-slmgr-vbs-options.md)
- [Esempio: Risoluzione dei problemi di attivazione dei client con attivazione basata su Active Directory](activation-troubleshoot-adba-clients.md)

Gli articoli seguenti includono indicazioni per la risoluzione di problemi di attivazione più specifici:

- [Risoluzione dei codici di errore di attivazione comuni](activation-error-codes.md)
- [Attivazione tramite il Servizio di gestione delle chiavi: problemi noti](activation-troubleshoot-KMS-issues.md)
- [Attivazione tramite codice ad attivazione multipla: problemi noti](activation-troubleshoot-MAK-issues.md)
- [Linee guida per la risoluzione dei problemi di attivazione correlati al DNS](common-troubleshooting-procedures-kms-dns.md)
- [Ricompilare il file Tokens.dat](activation-rebuild-tokens-dat-file.md)
