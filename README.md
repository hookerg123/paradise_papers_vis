# paradise_papers_vis

## Backend

Consists of neo4j database within a Docker image.

Current I am using a neo4j Sandbox. I explain my approach for loading the CSV data into neo4j below.

First I changed the headers of the CSV files so that the import tool would work e.g.
```
# paradise_papers.edges.csv
"node_id:START_ID","rel_type:TYPE","node_id:END_ID","link","start_date","end_date","sourceID","valid_until"

# paradise_papers.nodes.address.csv
"node_id:ID(Address)","name","address","country_codes","countries","sourceID","valid_until","note"

# paradise_papers.nodes.entity.csv
"node_id:ID(Entity)","name","jurisdiction","jurisdiction_description","country_codes","countries","incorporation_date","inactivation_date","struck_off_date","closed_date","ibcRUC","status","company_type","service_provider","sourceID","valid_until","note"

# paradise_papers.nodes.intermediary.csv
"node_id:ID(Intermediary)","name","country_codes","countries","sourceID","valid_until","note"

#paradise_papers.nodes.officer.csv
"node_id:ID(Officer)","name","country_codes","countries","status","sourceID","valid_until","note"

#paradise_papers.nodes.other.csv
"node_id:ID(Other)","name","country_codes","countries","sourceID","valid_until","note"
```

Then I ran this command inside the neo4j Docker image to load in the data
```
../bin/neo4j-admin import --database ParadisePapers \
     --nodes=paradise_papers.nodes.address.csv \
     --nodes=paradise_papers.nodes.entity.csv \
     --nodes=paradise_papers.nodes.intermediary.csv \
     --nodes=paradise_papers.nodes.officer.csv \
     --nodes=paradise_papers.nodes.other.csv \
     --relationships=paradise_papers.edges.csv \
     --ignore-empty-strings=true \
     --skip-duplicate-nodes=true \
     --skip-bad-relationships=true \
     --bad-tolerance=1500 \
     --multiline-fields=true
```

I was able to load the data in but ran into an issue where the version of neo4j Bolt protocol was not supported by neovis.js (? Not too sure) and am currently unable to detect the database in the browser now.

## Frontend Client

All you need to do is open `index.html`!

Currently it is connected to a neo4j Sandbox as I could not resolve the neo4j database issues I had in the end.

Credentials can be changed inside `index.html` if you would like to connect to a different database.
