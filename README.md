# HowToCreateAlertsViaAPI
Start grafana you can do this with this tutorial

**[Installing Grafana](https://github.com/jeti20/Grafana-AWS)**


Then follow this insctruction to generate token 

StackOverflow: **[Grafana API Keys option not available](https://community.grafana.com/t/issue-trying-to-generate-api-keys/102544)**

Grafana: **[Service account tokens](https://grafana.com/docs/grafana/latest/administration/service-accounts/#to-create-a-service-account)**

You can check documentation about this

**[Alerting provisioning HTTP API](https://grafana.com/docs/grafana/latest/developers/http_api/alerting_provisioning/)**

**[Provision Alerting resources](https://grafana.com/docs/grafana/latest/alerting/set-up/provision-alerting-resources/)**


I managed to add one alert with this json.

```
{
	"title": "aaaaa",
	"ruleGroup": "TestowyFolder",
	"folderUID": "edzsq8u89qjuoc",
	"noDataState": "OK",
	"execErrState": "OK",
	"for": "5m",
	"orgId": 1,
	"uid": "",
	"condition": "B",
	"annotations": {
		"summary": "test_api_1"
	},
	"labels": {
		"API": "test1"
	},
	"data": [{
			"refId": "A",
			"relativeTimeRange": {
				"from": 600,
				"to": 0
			},
			"datasourceUid": "adzsq547id05ce",
			"model": {
				"intervalMs": 1000,
				"maxDataPoints": 43200,
				"refId": "A"
			}
		},
		{
			"refId": "B",
			"relativeTimeRange": {
				"from": 0,
				"to": 0
			},
			"datasourceUid": "__expr__",
			"model": {
				"conditions": [{
					"evaluator": {
						"params": [],
						"type": "gt"
					},
					"operator": {
						"type": "and"
					},
					"query": {
						"params": [
							"B"
						]
					},
					"reducer": {
						"params": [],
						"type": "last"
					},
					"type": "query"
				}],
				"datasource": {
					"type": "__expr__",
					"uid": "__expr__"
				},
				"expression": "A",
				"intervalMs": 1000,
				"maxDataPoints": 43200,
				"reducer": "sum",
				"refId": "B",
				"settings": {
					"mode": "dropNN"
				},
				"type": "reduce"
			}
		},
		{
			"refId": "C",
			"relativeTimeRange": {
				"from": 0,
				"to": 0
			},
			"datasourceUid": "-100",
			"model": {
				"conditions": [{
					"evaluator": {
						"params": [
							0
						],
						"type": "gt"
					},
					"operator": {
						"type": "and"
					},
					"query": {
						"params": [
							"C"
						]
					},
					"reducer": {
						"params": [],
						"type": "last"
					},
					"type": "query"
				}],
				"datasource": {
					"type": "__expr__",
					"uid": "__expr__"
				},
				"expression": "B",
				"intervalMs": 1000,
				"maxDataPoints": 43200,
				"refId": "C",
				"type": "threshold"
			}
		}
	]
}
```
I saved this json into a file data.json then performed this in console

```
curl -X POST "http://YOURIP:3000/api/v1/provisioning/alert-rules" -H "Content-Type: application/json" -H "Authorization: Bearer <twój token>" -d @data.json
```

JSON z komentarzami

```
{
	"title": "aaaaa", -> Nazwa alertu
	"ruleGroup": "TestowyFolder", -> Folder
	"folderUID": "edzsq8u89qjuoc", - folder uid.  -> nizej jest jak znaleźć
	"noDataState": "OK", -> Configure no data and error handling
	"execErrState": "OK", -> Configure no data and error handling
	"for": "5m", -> Pending period
	"orgId": 1, -> ID organizacji
	"uid": "", -> Unikalny identyfikator reguły alertu (można zostawić pusty, a system automatycznie go wygeneruje).
	"condition": "B", ->Odniesienie do warunku, na którym opiera się alert
	"annotations": {
		"summary": "test_api_1"
	},
	"labels": {
		"API": "test1"
	},
	"data": [{ -> Zawiera listę zapytań, na podstawie których alert zostaje wyzwolony
			"refId": "A", -> Zapytanie A
			"relativeTimeRange": { -> Zakres czasowy
				"from": 600,
				"to": 0
			},
			"datasourceUid": "adzsq547id05ce", -> nizej jest jak znaleźć
			"model": {
				"intervalMs": 1000,
				"maxDataPoints": 43200,
				"refId": "A"
			}
		},
		{
			"refId": "B",
			"relativeTimeRange": {
				"from": 0,
				"to": 0
			},
			"datasourceUid": "__expr__",
			"model": {
				"conditions": [{
					"evaluator": {
						"params": [],
						"type": "gt"
					},
					"operator": {
						"type": "and"
					},
					"query": {
						"params": [
							"B"
						]
					},
					"reducer": {
						"params": [],
						"type": "last"
					},
					"type": "query"
				}],
				"datasource": {
					"type": "__expr__",
					"uid": "__expr__"
				},
				"expression": "A",
				"intervalMs": 1000,
				"maxDataPoints": 43200,
				"reducer": "sum",
				"refId": "B",
				"settings": {
					"mode": "dropNN"
				},
				"type": "reduce"
			}
		},
		{
			"refId": "C",
			"relativeTimeRange": {
				"from": 0,
				"to": 0
			},
			"datasourceUid": "-100", -> Wartość -100 jest specjalnym przypadkiem, który odnosi się do „wyrażenia” (ang. expression). Użycie -100 jako datasourceUid oznacza, że reguła alertu nie jest bezpośrednio powiązana z tradycyjnym źródłem danych, ale korzysta z wyrażenia, które może być obliczane na podstawie innych zapytań lub wyników
			"model": {
				"conditions": [{
					"evaluator": {
						"params": [
							0
						],
						"type": "gt"
					},
					"operator": {
						"type": "and"
					},
					"query": {
						"params": [
							"C"
						]
					},
					"reducer": {
						"params": [],
						"type": "last"
					},
					"type": "query"
				}],
				"datasource": {
					"type": "__expr__",
					"uid": "__expr__"
				},
				"expression": "B",
				"intervalMs": 1000,
				"maxDataPoints": 43200,
				"refId": "C",
				"type": "threshold"
			}
		}
	]
}
```
**[Jak wyciągnąć UID folderu](https://grafana.com/docs/grafana/latest/developers/http_api/folder/)**

**[Jak wyciągnąć UID datasorce](https://grafana.com/docs/grafana/latest/developers/http_api/data_source/)**
