# Example to read CKineticsDB JSON metadata file and make selections


```python
import json
import pandas as pd
```


```python
metadata_file = '../CKineticsDB_metadata.json'
```

## Open metadata file

Contains the top-level criteria of publication, species, and reactions. Selections should be made in only one criterion for one run.


```python
with open(metdata_file) as f:
    contents = json.loads(f.read())
contents.keys()
```




    dict_keys(['publications', 'species', 'reactions'])



## One of the ways to make selections is by using the pandas package

This is not a required method but is used here for explanation purposes.


```python
df = pd.read_json(contents['publications']).T
df = df[df['project'].isin(["Rh-ReOx - Ethylene Hydroformylation"])]
```

## Maintain the structure of the original JSON file in the selection criteria


```python
selected = df.to_json(orient = 'index')
```

## Write the selected contents to a new JSON file 

The file to be read by CKineticsDB for download must have the name 'CKineticsDB_metadata.json'. To ensure this:

1. Delete or rename the old CKineticsDB_metadata.json file.
2. Write the selected contents to the file named CKineticsDB_metadata.json.


```python
with open('../CKineticsDB_metadata.json', 'w+') as f:
    f.write(json.dumps(selected))
```

## Place the file in the destination directory, check the download instructions in the download.json file, and run the download command
