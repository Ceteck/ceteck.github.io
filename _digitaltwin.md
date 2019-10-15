## Digital Twin

The Digital Twin services allows to model a virtual replica of an asset or process.

### Import Asset Database
Via this route users can load a XML containing the definition of the asset model into a Database.

#### Request

The request contains multiple fields – required and non-required ones.

Use the following header: `Content-Type: text/xml`

```http
POST /v1/assetdatabases/{databaseId}/import HTTP/1.1
```

```http
HTTP/1.1 200 Database imported.
```

**Request URL:** `POST /v1/assetdatabases/{databaseId}/import`

#### Transfer Parameters
|Name           | Required | Parameter Type | Object Type | Description                                              |
|---------------|----------|----------------|-------------|----------------------------------------------------------|
| databaseId    | Yes      | Resource       | string      | Database ID                                              |
| xml           | Yes      | Body           | string      | XML containing the asset model                           |


### Sample XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<AM>
  <Name>Test Plant</Name>
  <AMElement>
    <Name>Production line 1</Name>
    <AMAttribute>
      <Name>Temperature</Name>
      <Value>30</Value>
    </AMAttribute>
    <AMAttribute>
      <Name>OEE</Name>
      <Value>75</Value>
    </AMAttribute>
  </AMElement>
  <AMElement>
    <Name>Production line 2</Name>
  </AMElement>
</AM>
```

#### Response

The response does not contain any content in the success case. The API answers with a HTTP status code of `200 OK`.

#### Common Return Codes
| Code | Description           |
|------|-----------------------|
| 200  | Database imported     |
| 400  | Malformed request     |
| 403  | Forbidden             |
| 500  | Server error          |


### Export Asset Database

Via this route users can export an asset model to a XML file.

#### Request

The request contains multiple fields – required and non-required ones.

```http
GET /v1/assetdatabases/{databaseId}/export HTTP/1.1
```

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="utf-8"?>
<AM>
  <Id>00001</Id>
  <Name>Test Plant</Name>
  <AMElement>
    <Id>00002</Id>
    <Name>Production line 1</Name>
    <AMAttribute>
      <Id>00003</Id>
      <Name>Temperature</Name>
      <Value>30</Value>
    </AMAttribute>
    <AMAttribute>
      <Id>00004</Id>
      <Name>OEE</Name>
      <Value>75</Value>
    </AMAttribute>
  </AMElement>
  <AMElement>
    <Id>00005</Id>
    <Name>Production line 2</Name>
  </AMElement>
</AM>
```

**Request URL:** `GET /v1/assetdatabases/{databaseId}/import`

#### Transfer Parameters
|Name           | Required | Parameter Type | Object Type | Description                                              |
|---------------|----------|----------------|-------------|----------------------------------------------------------|
| databaseId    | Yes      | Resource       | string      | Database ID                                              |


#### Response
OK Database exported. The response body contains the serialized database.

#### Common Return Codes
| Code | Description           |
|------|-----------------------|
| 200  | Database exported     |
| 400  | Malformed request     |
| 403  | Forbidden             |
| 500  | Server error          |


### Create a new analysis on a node
Via this route users can create new analysis on a node.

#### Request

The request contains multiple fields – required and non-required ones.

```http
POST /v1/nodes/{nodeId}/analyses HTTP/1.1
```

```http
HTTP/1.1 201 The Analysis was created.
Location:  /v1/nodes/1003/analyses/435

{
    "id": "435",
    "name": "Tank Level Target",
    "description": "Description of the analysis",
    "enabled": true,
    "configString": "def find_max (L): if len(L) == 1: return L[0] v1 = L[0] v2 = find_max(L[1:]) if v1 > v2: return v1 else: return v2",
    "variableMapping": "L:=|Level; Output=|Level Forecast",
    "frequency": 960,
    "needTraining": false,
    "retrainingFrequency": 0
}
```

**Request URL:** `POST /v1/nodes/{nodeId}/analyses`

#### Transfer Parameters
|Name              | Required | Parameter Type | Object Type | Description                                              |
|------------------|----------|----------------|-------------|----------------------------------------------------------|
| nodeId           | Yes      | Resource       | string      | Node     ID                                              |
| name             | Yes      | Body           | string      | Name of the analysis                                     |
| description      | No       | Body           | string      | Description of the analysis                              |
| enabled          | No       | Body           | boolean     | Enabled or disabled                                      |
| config_string    | No       | Body           | string      | Implementation in python of the algorithm to calculate the analysis |
| variable_mapping | No       | Body           | string      | Configures the mapping of the variables defined in the config_string. i.e: "b&#124;&#124;Attribute1;c&#124;&#124;Attribute2" |
| frequency        | Yes      | Body           | int         | Calculation frequency, in seconds, of the mathematical model script |
| need_retraining  | No       | Body           | boolean     | `true` or `false`                                        |
| retraining_freq  | No       | Body           | int         | Retraining frecuency                                     |


#### Response

The response contains the order-object in a success case. The API answers with a HTTP status code of 201 Created. Additionally the Location-header contains a relative URI to the order-object.

#### Common Return Codes
| Code | Description           |
|------|-----------------------|
| 201  | Analysis created      |
| 400  | Malformed request     |
| 403  | Forbidden             |
| 500  | Server error          |

