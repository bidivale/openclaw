# Disabled onedrive by masking it - as simple disabling was not working 

systemctl --user mask onedrive.service

systemctl --user status onedrive



# If I want to use it in the future I need to run the following 

systemctl --user unmask onedrive.service
systemctl --user start onedrive
