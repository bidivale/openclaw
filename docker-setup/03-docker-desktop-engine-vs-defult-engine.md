# I have used docker desktop engine and built all the images and containers inside that 

# Native docker engine was not running 

# Then I started native docker demaon 
sudo systemctl start docker

sudo systemctl status docker


# Then I switched Docker CLI to the native engine
docker context use default


# Then I checked the current engine
docker context ls

# result 
default *
desktop-linux

# Then I checked whether my all the heavy images and containers are still there in default engine
docker ps

But they are not


# Now what I want is to safly copy and run all the images and volume that is in my docker desktop to default engine
Do not delete anything from docker desktop or its engine yet. Some are very large images that I do not want to build again. Just make sure everything (all the containers) is running from docker default engine without using docker desktop or its engine. 
Can you do that safely?