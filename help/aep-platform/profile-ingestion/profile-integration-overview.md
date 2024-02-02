---
title: "[!DNL Platform] Översikt över integreringshandboken för profilregistrering och åtkomst"
description: Läs om integrationen för [!DNL Experience Platform] Intag och åtkomst av profiler.
exl-id: a593511c-dd4c-4437-af73-f44d795cacb8
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# Integreringsguide: [!DNL Experience Platform] profilintag och åtkomst

Partners bör använda den här integreringsguiden för att hjälpa dem att bygga ut ingress- och utgångfunktioner med Adobe [!DNL Experience Platform] (AEP) Det finns API:er för gruppinmatning, direktuppspelning och enhetlig profilåtkomst (egress).

För att bidra till utvecklingen ska [Postman Collection](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) har skapats av Adobe Exchange-teamet. Det finns referenser till den här Postman-samlingen i hela integreringsguiden.

Mer information om hur du installerar och använder Postman-samlingen finns på Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) sida. Det finns också exempeldatauppsättningar med [lojalitet](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) och [profil](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) data.

## Exempel på användning av integrering: Interaktivt röstsvarssystem

För integratörer [!DNL Experience Platform] API:er har alla funktioner på plattformen som finns i användargränssnittet, men nu med möjligheten att skapa kundarbetsflöden och automatiserade dataflöden. Som integratör kontrollerar ni regelbundet datauppsättningens status, skapar nya rutiner för datainhämtning och integrerar er egen målgruppsaktiveringslösning med den enhetliga profilen.

Ta en titt på världen med interaktiva Voice Response-system (IVR) och hanteringsprogram för Call Center. Leverantören kan använda [!DNL Experience Platform] API:er för inhämtning av historisk information om kundens callcenteraktivitet i Experience Data Lake. Om data har importerats i XDM `ExperienceEvent` Schema (ett schema som uttrycker kundinteraktioner), kan dessa interaktioner infogas utan friktion direkt i den enhetliga profiltjänsten. I det här fallet `callerId` används som kundens identifierare. Identitetstjänsten tar hand om identitetsmatchning och hjälper den enhetliga profiltjänsten att lägga till datapunkter från nyligen använda interaktioner med Call Center till kundens profil.

Nästa gång en kund ringer till Call Center besvaras de först av IVR. För att personalisera meddelandet och leverera ett erbjudande som är skräddarsytt för anroparen måste IVR-systemet förstå mer om anroparen. Det är här som API-integrering med den enhetliga profilen kommer in. IVR-backend kan kontakta tjänsten för enhetlig profil för att få en punktsökning. Läs sedan antingen profilattributen som gäller endast kundtjänstinteraktionen eller hela kundprofilen, som även har attribut för interaktion på andra kontaktytor. Genom att kombinera data från flera datakällor, använda identitetsupplösning och den enhetliga profilen, kan call center och IVR-leverantören leverera en skräddarsydd kundupplevelse som stöds av Adobe [!DNL Experience Platform].&quot;

## Allmänna resurser

* AEP [Produktdokumentation](https://docs.adobe.com/content/help/en/experience-platform/landing/documentation/overview.html).
* AEP [Utbyggbarhet](https://www.adobe.com/insights/experience-platform-api-extensibility.html).

## Frågor eller feedback?

Skicka alla frågor och feedback via [supportkanal](https://adobeexchangeec.zendesk.com/hc/se-sv/requests/new)
