---
title: Import i realtid
description: Lär dig importera data till AEP i realtid.
exl-id: 0b6215a9-1160-49ae-8aa5-302b47357200
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# Strömma data till AEP

Adobe [!DNL Experience Platform] gör att både profil- och upplevelsehändelser kan direktuppspelas och vara tillgängliga i nära realtid. Alla data som skickas till AEP via direktuppspelning bevaras i sjön med data. Data kan direktuppspelas till befintliga datauppsättningar eller till helt nya datauppsättningar via API:er eller med Adobe Launch.

Den här artikeln kommer att omfatta följande:

* Direktuppspelning till den enskilda XDM-profilen
* Direktuppspelning till XDM ExperienceEvent
* Använda AEP-tillägget Launch för direktuppspelning

The [Postman Collection](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) refereras till i hela artikeln med tillhörande anrop per nummer. Mer information om hur du installerar och använder Postman-samlingen finns på Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) sida. Det finns också exempeldatauppsättningar med [lojalitet](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) och [profil](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) data.

## Förutsättningar

* [Autentisera till plattformen](https://docs.adobe.com/content/help/en/experience-platform/tutorials/authentication.html).
* Samla in värden för obligatoriska rubriker från den autentiseringssjälvstudiekurs som är länkad ovan.

## Skapa en direktuppspelningsanslutning

Om du vill direktuppspela till AEP måste du först skapa en direktuppspelningsanslutning. Direktuppspelningsanslutningar innehåller attribut som källan till direktuppspelningsdata och huruvida du skickar i poster som tillhör [!DNL Experience Data Model] (XDM) scheman. När du har skapat en direktuppspelningsanslutning får du en unik URL som du använder för att strömma data till AEP.

Gå [här](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/create-streaming-connection.html) för instruktioner om hur du skapar en direktuppspelningsanslutning via API eller [här](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/create-streaming-connection-ui.html) för instruktioner om hur du skapar en direktuppspelningsanslutning via användargränssnittet.

```json
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "Sample name",
     "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
     "description": "Sample description",
     "connectionSpec": {
         "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
         "version": "1.0"
     },
     "auth": {
         "specName": "Streaming Connection",
         "params": {
             "sourceId": "Sample connection",
             "dataType": "xdm",
             "name": "Sample connection"
         }
     }
 }
```

Svar:

```json
 {
    "id": "77a05521-91d6-451c-a055-2191d6851c34",
    "etag": "\"a500e689-0000-0200-0000-5e31df730000\""
}
```

Var noga med att spara det ID som anges i svaret ovan för framtida direktuppspelningsanrop (Postman-samlingen sparar detta åt dig i miljövariabeln CONNECTION_ID).

## Strömma profildata till AEP

I det här avsnittet använder du Postman anropsmappar: 3: Import i realtid, 3a: Import i realtid av PROFILE-data.

Detaljerade JSON-begäranden med svar för strömmande profildata dokumenteras [här](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/streaming-record-data.html).

Steg:

1. Skapa ett enskilt XDM-profilschema
1. Ange primär identitetsbeskrivning för enskild XDM-profil (primärnyckel)
1. Skapa en datauppsättning för enskilda XDM-profilposter
1. Anropa API:erna för direktuppspelningsinmatning för att skapa en enskild XDM-profilpost
1. Hämta den nyligen skapade profilen

## Strömma upplevelsehändelser till AEP

I det här avsnittet använder du Postman anropsmappar: 3: Import i realtid, 3b: Import i realtid av PROFILE-data.

Detaljerade JSON-förfrågningar med svar för strömmande upplevelsedata dokumenteras [här](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/streaming-time-series-data.html).

Steg:

1. Skapa ett XDM ExperienceEvent-schema
1. Ange den primära identitetsbeskrivningen för XDM ExperienceEvent (primärnyckel)
1. Skapa en datauppsättning för XDM ExperienceEvents
1. Anropa API:erna för direktuppspelningsinmatning för att skapa en XDM ExperienceEvent
1. Hämta den nyligen skapade händelsen

## Använd Experience Platform-taggar för direktuppspelning till AEP

Adobe [!DNL Experience Platform] Med Launch-tillägget kan du direktuppspela till AEP via Launch. Mer information finns på [den här guiden](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html).

## Referensartiklar

* [API:er för datainmatning](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/acpdr/swagger-specs)
* [Direktuppspelning - översikt](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/streaming_ingest_overview.md)
* [Utvecklarhandbok för direktuppspelning av inmatningsmaterial](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/getting_started_with_platform_streaming_ingestion.md)
* [Använda AEP Launch Extension](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html)
