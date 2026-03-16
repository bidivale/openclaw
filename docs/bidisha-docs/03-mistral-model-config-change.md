# In the onboarding configuration in the doc 03-dockersetp-terminal-QA.md - the model was misconfugred.

## problem

The configured model is ministral-8b-latest but the models list in config only defines mistral-large-latest. ministral-8b-latest isn't in the list,

## Quickly fixed by adding ministral-8b-latest to my models config

docker compose exec openclaw-gateway node dist/index.js config set \
 models.providers.mistral.models \
 '[{"id":"mistral-large-latest","name":"Mistral Large","reasoning":false,"input":["text","image"],"cost":{"input":0,"output":0,"cacheRead":0,"cacheWrite":0},"contextWindow":262144,"maxTokens":262144},{"id":"ministral-8b-latest","name":"Ministral 8B","reasoning":false,"input":["text"],"cost":{"input":0,"output":0,"cacheRead":0,"cacheWrite":0},"contextWindow":131072,"maxTokens":131072}]'


 ## restart the container after that 
 docker compose restart openclaw-gateway

 ## model changed to mistral large 
 docker compose exec openclaw-gateway node dist/index.js config set agents.defaults.model '{"primary":"mistral/mistral-large-latest"}'
