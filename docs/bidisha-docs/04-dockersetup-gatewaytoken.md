# after the docker setup run - we got 2 commands in the terminal 

## command 1 - health check
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml logs -f openclaw-gateway

# command 2 - gateway api token setup
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml exec openclaw-gateway node dist/index.js health --token "24a7521875fd9179f133eb4d496e99b7b60d59b01266acb961645ff28df1db53"

# command 2 did not work - so I tried this - and it worked 
 docker compose -f docker-compose.yml exec openclaw-gateway node dist/index.js gateway health --token "24a7521875fd9179f133eb4d496e99b7b60d59b01266acb961645ff28df1db53"
