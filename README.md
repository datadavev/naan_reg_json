# naan_reg_json

`naan_reg_json` provides a tool for converting NAAN registry ANVL to JSON. It 
includes JSON-schema with generated docuemntation for private and publicly 
visible NAAN records.


## Usage

```
$python naan_reg_json.py --help
usage: naan_reg_json.py [-h] [-s] [-p] [path]

Generate JSON representation of naan_reg_priv/main_naans. This tool translates the 
NAAN registry file from ANVL to JSON to assist downstream programmatic use. If pydantic 
is installed then the script can output JSON schema describing the JSON respresentation 
of the transformed NAAN entries.

positional arguments:
  path          Path to naan_reg_priv ANVL file.

optional arguments:
  -h, --help    show this help message and exit
  -s, --schema  Generate JSON schema and exit.
  -p, --public  Output public content only.
```

For conversion from ANVL to JSON no additional dependencies are needed beyond Python3.8+. E.g. (portions redacted):

```
$python naan_reg_json.py ../naan_reg_priv/main_naans

{
  "12025": {
    "what": "12025",
    "where": "http://www.nlm.nih.gov",
    "target": "http://www.nlm.nih.gov/$arkpid",
    "when": "2001-03-08T00:00:00+00:00",
    "who": {
      "name": "US National Library of Medicine",
      "acronym": null,
      "address": "#REDACTED#"
    },
    "na_policy": {
      "orgtype": "NP",
      "policy": "(:unkn) unknown",
      "tenure": "2001",
      "policy_url": null
    },
    "why": "ARK",
    "contact": {
      "name": "#REDACTED#",
      "unit": null,
      "tenure": null,
      "email": "#REDACTED#",
      "phone": "#REDACTED#"
    },
    "comments": null,
    "provider": null
  },
  ...
```

A public view of the ANVL to JSON can be generated using the `-p` or `--public` option:

```
$python naan_reg_json.py -p ../naan_reg_priv/main_naans

{
  "12025": {
    "what": "12025",
    "where": "http://www.nlm.nih.gov",
    "target": "http://www.nlm.nih.gov/$arkpid",
    "when": "2001-03-08T00:00:00+00:00",
    "who": {
      "name": "US National Library of Medicine",
      "acronym": null
    },
    "na_policy": {
      "orgtype": "NP",
      "policy": "(:unkn) unknown",
      "tenure": "2001",
      "policy_url": null
    }
  },
```

For JSON-schema generation, `pydantic` is required:
```
pip install pydantic
```

The public view and full view have different schemas, they can be generated like:
```
$python naan_reg_json.py -s > schema/naan_schema.json
```
and
```
$python naan_reg_json.py -s -p > schema/naan_schema_public.json
```

Schema documentation can be generated if `json-schema-for-humans` is installed:
```
pip install json-schema-for-humans
```

To generate the schema documentation:
```
generate-schema-doc --config-file docs_config.yaml ./schema ./schema/
```

## Acknowledgement

This work is supported in part by the California Digital Library.

