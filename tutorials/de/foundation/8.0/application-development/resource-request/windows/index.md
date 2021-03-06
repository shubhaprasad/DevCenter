---
layout: tutorial
title: Ressourcenanforderung von Windows-Anwendungen
breadcrumb_title: Windows
relevantTo: [windows]
downloads:
  - name: Natives Windows-8-Projekt herunterladen
    url: https://github.com/MobileFirst-Platform-Developer-Center/ResourceRequestWin8/tree/release80
  - name: Natives Windows-10-Projekt herunterladen
    url: https://github.com/MobileFirst-Platform-Developer-Center/ResourceRequestWin10/tree/release80
  - name: Adapter-Maven-Projekt herunterladen
    url: https://github.com/MobileFirst-Platform-Developer-Center/Adapters/tree/release80
weight: 6
---
<!-- NLS_CHARSET=UTF-8 -->
## Übersicht
{: #overview }
Mit der {{ site.data.keys.mf_console }} erstellte Anwendungen können mit der REST-API `WorklightResourceRequest` auf Ressourcen zugreifen.   
Die REST-API funktioniert mit allen Adaptern und externen Ressourcen. 

**Voraussetzungen:**

- Stellen Sie sicher, dass das SDK der {{ site.data.keys.product }} zu Ihrer nativen [universellen Windows-8.1- oder Windows-10-UWP-Anwendung](../../../application-development/sdk/windows-8-10) hinzugefügt wurde. 
- Informieren Sie sich über das [Erstellen von Adaptern](../../../adapters/creating-adapters/).

## WLResourceRequest
{: #wlresourcerequest }
Die Klasse `WorklightResourceRequest` handhabt an Adapter oder externe Ressourcen gerichtete Ressourcenanforderungen. 

Erstellen Sie ein `WorklightResourceRequest`-Objekt und geben Sie den Pfad zu der Ressource und die HTTP-Methode an.   
Verfügbare Methoden sind `GET`, `POST`, `PUT` und `DELETE`.

```cs
URI adapterPath = new URI("/adapters/JavaAdapter/users",UriKind.Relative);
WorklightResourceRequest request = WorklightClient.ResourceRequest(adapterPath,"GET");
```

* Verwenden Sie für **JavaScript-Adapter** `/adapters/{AdapterName}/{procedureName}`. 
* Verwenden Sie für **Java-Adapter** `/adapters/{AdapterName}/{path}`. Die Angabe für `path` hängt davon ab, wie Sie Ihre
`@Path`-Annotationen im Java-Code definiert haben. Eingeschlossen sind auch alle verwendeten `@PathParam`-Annotationen. 
* Wenn Sie auf Ressourcen außerhalb des Projekts zugreifen möchten, verwenden Sie die vollständige URL nach Maßgabe des externen Servers. 
* **timeout**: Anforderungszeitlimit in Millisekunden (optional)
* **scope**: Optional, wenn Sie wissen, mit welchem Bereich die Ressource geschützt wird. Durch Angabe dieses Bereichs kann die Abfrage effizienter werden. 

## Anforderung senden
{: #sending-the-request }
Fordern Sie die Ressource mit der Methode `.send()` an. 

```cs
WorklightResponse response = await request.send();
```

Verwenden Sie das Objekt `WorklightResponse response`, um die vom Adapter abgerufenen Daten zu erhalten. 

Das Objekt `response` enthält die Antwortdaten. Über die Methoden und Eigenschaften dieses Objekts können Sie die erforderlichen Informationen abrufen. Gängige Eigenschaften sind
`ResponseText`, `ResponseJSON` (wenn die Antwort im JSON-Format vorliegt) , `Success`
(zur Angabe, ob der Aufruf erfolgreich war oder fehlgeschlagen ist) und `HTTPStatus` (HTTP-Status der Antwort). 

## Parameter
{: #parameters }
Bevor Sie Ihre Anforderung senden, können Sie nach Bedarf Parameter hinzufügen. 

### Pfadparameter
{: #path-parameters }
Pfadparameter (`/path/value1/value2`) werden - wie bereits erläutert - während der Erstellung des `WorklightResourceRequest`-Objekts festgelegt: 

```cs
Uri adapterPath = new Uri("/adapters/JavaAdapter/users/value1/value2",UriKind.Relative);
WorklightResourceRequest request = WorklightClient.createInstance(adapterPath,"GET");
```

### Abfrageparameter
{: #query-parameters }
Wenn Sie Abfrageparameter (`/path?param1=value1...`) senden möchten, verwenden Sie für die einzelnen Parameter die Methode `SetQueryParameter`: 

```cs
request.SetQueryParameter("param1","value1");
request.SetQueryParameter("param2","value2");
```

#### JavaScript-Adapter
{: #javascript-adapters-query }
JavaScript-Adapter verwenden sortierte unbenannte Parameter. Wenn Sie Parameter an einen JavaScript-Adapter übergeben möchten, definieren Sie ein Parameter-Array mit dem Namen `params`:

```cs
request.SetQueryParameter("params","['value1', 'value2']");
```

Dieses Array sollte mit `GET` verwendet werden.

### Formularparameter
{: #form-parameters }
Wenn Sie im Hauptteil Formularparameter senden möchten, verwenden Sie
`.Send(Dictionary<string, string> formParameters)` anstelle von `.Send()`:  

```cs
Dictionary<string,string> formParams = new Dictionary<string,string>();
formParams.Add("height", height.getText().toString());
request.Send(formParams);
```   

#### JavaScript-Adapter
JavaScript-Adapter verwenden sortierte unbenannte Parameter. Wenn Sie Parameter an einen JavaScript-Adapter übergeben möchten, definieren Sie ein Parameter-Array mit dem Namen `params`:

```cs
formParams.Add("params","['value1', 'value2']");
```

Dieses Array sollte mit `POST` verwendet werden.

### Headerparameter
{: #header-parameters }
Wenn Sie einen Parameter als HTTP-Header senden möchten, verwenden Sie die API `.SetHeader()`: 

```cs
request.SetHeader(KeyValuePair<string,string> header);
```

### Weitere angepasste Hauptteilparameter
{: #other-custom-body-parameters }
- Mit `.Send(requestBody)` können Sie im Hauptteil eine beliebige Zeichenfolge festlegen. 
- Mit `.Send(JObject json)` können Sie im Hauptteil ein beliebiges Verzeichnis festlegen. 
- Mit `.Send(byte[] data)` können Sie im Hauptteil ein beliebiges Byte-Array festlegen. 

## Antwort
{: #the-response }
Das `WorklightResponse`-Objekt enthält die Antwortdaten. Über die Methoden und Eigenschaften dieses Objekts können Sie die erforderlichen Informationen abrufen. Gängige Eigenschaften sind
`ResponseText` (String), `ResponseJSON` (JSONObject) (wenn die Antwort im JSON-Format vorliegt)
und `success` (Boolean) (für den Erfolgsstatus der Antwort). 

Falls eine Anforderung fehlschlägt, enthält das Antwortobjekt auch eine Eigenschaft `error`. 

## Weitere Informationen
{: #for-more-information }
> Weitere Hinweise zu WLResourceRequest finden Sie in den [API-Referenzinformationen](http://public.dhe.ibm.com/software/products/en/MobileFirstPlatform/docs/v800/mfpf_csharp_win8_native_client_api.pdf).

<img alt="Beispielanwendung" src="resource-request-success-win8-10.png" style="float:right"/>
## Beispielanwendung
{: #sample-application }
Die Projekte ResourceRequestWin8 und ResourceRequestWin10 enthalten jeweils eine native universelle Windows-8-Anwendung bzw. Windows-10-UWP-Anwendung, die mit einem Java-Adapter eine Ressourcenanforderung absetzt.   
Das Adapter-Maven-Projekt enthält den beim Aufrufen der Ressourcenanforderung verwendeten Java-Adapter. 

[Klicken Sie hier](https://github.com/MobileFirst-Platform-Developer-Center/ResourceRequestWin8/tree/release80), um das universelle Windows-8.1-Projekt herunterzuladen.   
[Klicken Sie hier](https://github.com/MobileFirst-Platform-Developer-Center/ResourceRequestWin10/tree/release80), um das Windows-10-UWP-Projekt herunterzuladen.   
[Klicken Sie hier](https://github.com/MobileFirst-Platform-Developer-Center/Adapters/tree/release80), um das Adapter-Maven-Projekt herunterzuladen. 

### Verwendung des Beispiels
{: #sample-usage }
Anweisungen finden Sie in der Datei README.md zum Beispiel. 
