Hello! We are building integration with Saperion system.

Ours integration URL: [https://hdmdmz1.human.de:8095/docgate](https://hdmdmz1.human.de:8095/docgate)

We use it, as a base URL for API requests to Saperion docgate.

Previously we used another base url, that was in internal network: http://hdm3/DocGate

You have provided example requests to it:
> # 1. Search Documents
> - use "SYSTIMESTAMP" to get all changes since the last run ("SYSTIMESTAMP" is UTC)
> `GET /api/dms/stores/ProduktDoku_Edit/documents?select=XDOCUMENTID,XHDOC,SYSROWID&filter=SYSTIMESTAMP GE '2023-01-01 00:00:00' and SYSTIMESTAMP LT '2023-01-02 00:00:00'&orderby=SYSTIMESTAMP asc`
> - fields you need in your result:
> - "XDOCUMENTID": ID you need to load the document and file content
> 
> "SYSROWID": "ROWID" of the document, Remains the same across all versions of the document — You can use this ID to assign the document
> "XHDOC": Changes with each version — You can use this ID to determine if the document has changed, you can also add other fields to the result list
> 
> # 2. Load the Document (for each item in the search result)
> `GET /api/dms/stores/ProduktDoku_Edit/documents/{XDOCUMENTID}`
> - you receive the document with the meta information of the files, here you can get the filename and the filed
> - a document can contain 0-n files
> 
> # 3. Get the files content (for each file of the documents)
> `GET /api/dms/stores/ProduktDoku_Edit/documents/{XDOCUMENTID}/files/{FileId}/content`
>- you receive the binary stream of the file
> We request an example of a request that would work with the current version of the server (as was originally attached to the example with a filter by date)

Now we use new API Url and make requests from outer network. And we encountered some problems with API.

First of all swagger documentation is broken on our instance. You may check it over that link: https://hdmdmz1.human.de/DocGate/swagger/ui/index

Because of that we used another documentation that we found over those URLs:

[https://docs.hyland.com/Saperion/javadocs/8.0/SP1/RESTAPI/ui/index.html#/](https://docs.hyland.com/Saperion/javadocs/8.0/SP1/RESTAPI/ui/index.html#/) and [https://docs.hyland.com/Saperion/javadocs/Foundation/EP1/RESTAPI/resource_DocumentResourceImpl.html](https://docs.hyland.com/Saperion/javadocs/Foundation/EP1/RESTAPI/resource_DocumentResourceImpl.html)

Second issue is about making request to get a list of the documents inside the Saperion System.

Previously we used a method  `GET API_URL/api/dms/stores/ProduktDoku_Edit/documents`, that is missing in documentation that we found now.

Requests that were working before are failing now, for example:
```bash
curl --location 'https://hdmdmz1.human.de:8095/docgate/api/dms/stores/ProduktDoku_Edit/documents?select=XDOCUMENTID%2CXHDOC%2CSYSROWID&filter=SYSTIMESTAMP%20GE%20%272023-01-01%2000%3A00%3A00%27%20and%20SYSTIMESTAMP%20LT%20%272023-01-02%2000%3A00%3A00%27&orderby=SYSTIMESTAMP%20asc' \
--header 'X-API-KEY: 87b393fc0a0025b77baece81e98173c2aee88b234bd5c4bb9bcc38d0c2322445'
```

But all requests to `/docgate/api/dms/stores/{storeName}/documents` return an error like:  
```json
{"Message": "An error has occurred."}
```

It is not possible to understand from the error what exactly is wrong.

There is an assumption that the version in the documentation with which we previously worked differs from the one that is available now. It would be nice to get access to the swagger portal built into the API, or get a couple of examples of working requests.

Another requests that we tried:
```bash
curl --location 'https://hdmdmz1.human.de:8095/docgate/api/dms/stores/ProduktDoku_Edit/documents?filter=SYSTIMESTAMP%20GE%20%272023-01-01%27' \
--header 'X-API-KEY: 87b393fc0a0025b77baece81e98173c2aee88b234bd5c4bb9bcc38d0c2322445'
```

