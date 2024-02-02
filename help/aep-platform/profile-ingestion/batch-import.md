---
title: Importera batchdata till AEP
description: Lär dig hur du importerar gruppfiler till Experience Platform
exl-id: 50576b67-b3ba-498e-86f6-7e1986b76985
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '458'
ht-degree: 0%

---

# Importera batchdata till AEP

AEP kan importera gruppfiler som innehåller profildata från en platt fil (t.ex. parquet) eller data som följer ett känt schema i [!UICONTROL Experience Data Model] (XDM)-registret.

AEP kan importera data med hjälp av gruppfiler. Följande format accepteras: JSON, Parquet och CSV.

Den här artikeln kommer att omfatta följande:

* Förutsättningar för gruppinmatning
* Bästa praxis och gränser för batchförbrukning
* Så här skapar du en batch
* Hur man slutför en batch
* Kontrollera status för en batch

The [Postman Collection](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) refereras till i hela artikeln med tillhörande anrop per nummer. Mer information om hur du installerar och använder Postman-samlingen finns på Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) sida. Det finns också exempeldatauppsättningar med [lojalitet](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) och [profil](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) data.

För alla anrop i den här självstudiekursen använder du Postman samtalsmappar: 4: Batchimport, 4a: Batchimport för PROFILE-data ELLER 4b: Batchimport för EVENT-data.

## Förutsättningar för gruppinmatning

* Definiera ett schema och skapa en datauppsättning.
* Data måste vara formaterade i JSON, Parquet eller CSV.
* [Autentisera till plattformen](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md).
* Samla in värden för obligatoriska rubriker från den autentiseringssjälvstudiekurs som är länkad ovan.

## Bästa praxis och gränser för batchförbrukning

* Maximal batchstorlek: 100 GB
* Maximalt antal filer per batch: 1 500
* Om en fil är större än 512 MB måste den delas upp i mindre segment. Mer information finns i [utvecklarhandbok](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
* Maximalt antal egenskaper eller fält per rad: 10 000
* Maximalt antal batchar per minut, per användare: 138

## Skapa en batch

I den här självstudiekursen kommer vi att använda JSON som format. Fler formatexempel finns i [utvecklarhandbok](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
Skapa en batch med JSON som indataformat (se till att inkludera ett datauppsättnings-ID och att dina data överensstämmer med det XDM-schema som är länkat till datauppsättningen):

```json
curl -X POST "https://platform.adobe.io/data/foundation/import/batches" \
  -H "Accept: application/json" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
  -d '{
          "datasetId": "{DATASET_ID}",
           "inputFormat": {
                "format": "json"
           }
      }'
```

Svar:

```json
{
    "id": "{BATCH_ID}",
    "imsOrg": "{IMS_ORG}",
    "updated": 0,
    "status": "loading",
    "created": 0,
    "relatedObjects": [
        {
            "type": "dataSet",
            "id": "{DATASET_ID}"
        }
    ],
    "version": "1.0.0",
    "tags": {},
    "createdUser": "{USER_ID}",
    "updatedUser": "{USER_ID}"
}
```

## Överför filer

Filer kan nu överföras till den nyligen skapade gruppen (med batch_id från svaret ovan).

```json
curl -X PUT "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.json" \
  -H "content-type: application/octet-stream" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}" \
  --data-binary "@{FILE_PATH_AND_NAME}.json"
```

>[!NOTE]
>
>API:t stöder endast överföring till en del, vilket innebär att varje fil/mikrobatch måste överföras med individuella anrop. Kontrollera att innehållstypen är application/octet-stream.

Svar:

```
200 OK
```

## Slutför en batch

När alla filer har överförts signalerar det här anropet att gruppen är klar för befordran:

```json
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
```

Svar:

```
200 OK
```

## Kontrollera status för en batch

Batchstatusen kan kontrolleras i användargränssnittet eller via API:t (se anropet nedan). Navigera till DataSet för att se statusen för att checka in användargränssnittet.

De olika statusvärdena för batchimporten kan hittas [här](https://adobe.ly/2TMMCmj).


```json
curl GET "https://platform.adobe.io/data/foundation/catalog/batch/{BATCH_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key: {API_KEY}"
```

Svar:

```json
{
    "{BATCH_ID}": {
        "imsOrg": "{IMS_ORG}",
        "created": 1494349962314,
        "createdClient": "MCDPCatalogService",
        "createdUser": "{USER_ID}",
        "updatedUser": "{USER_ID}",
        "updated": 1494349963467,
        "externalId": "{EXTERNAL_ID}",
        "status": "success",
        "errors": [
            {
                "code": "err-1494349963436"
            }
        ],
        "version": "1.0.3",
        "availableDates": {
            "startDate": 1337,
            "endDate": 4000
        },
        "relatedObjects": [
            {
                "type": "batch",
                "id": "foo_batch"
            },
            {
                "type": "connection",
                "id": "foo_connection"
            },
            {
                "type": "connector",
                "id": "foo_connector"
            },
            {
                "type": "dataSet",
                "id": "foo_dataSet"
            },
            {
                "type": "dataSetView",
                "id": "foo_dataSetView"
            },
            {
                "type": "dataSetFile",
                "id": "foo_dataSetFile"
            },
            {
                "type": "expressionBlock",
                "id": "foo_expressionBlock"
            },
            {
                "type": "service",
                "id": "foo_service"
            },
            {
                "type": "serviceDefinition",
                "id": "foo_serviceDefinition"
            }
        ],
        "metrics": {
            "foo": 1337
        },
        "tags": {
            "foo_bar": [
                "stuff"
            ],
            "bar_foo": [
                "woo",
                "baz"
            ],
            "foo/bar/foo-bar": [
                "weehaw",
                "wee:haw"
            ]
        },
        "inputFormat": {
            "format": "parquet",
            "delimiter": ".",
            "quote": "`",
            "escape": "\\",
            "nullMarker": "",
            "header": "true",
            "charset": "UTF-8"
        }
    }
}
```

## Referensartiklar

* [API för datainmatning](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/acpdr/swagger-specs)
* [Översikt över batchförbrukning](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/ingest_architectural_overview.md)
* [Utvecklarhandbok för batchintag](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
* [Felsökningsguide för batchinmatning](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_troubleshooting_guide.md)
* [Inmatning av data i Postman Collection](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Data%20Ingestion%20API.postman_collection.json)
* [Självstudiekurs om autentisering](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md)
