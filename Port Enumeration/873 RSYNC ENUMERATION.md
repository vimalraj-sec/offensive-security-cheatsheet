```md
# Tool Used
nc
rsync

# INTERACT
sudo rsync $ip::

# LIST SHARES
rsync -av --list-only rsync://$ip

# DOWNLOAD CONTENT FROM SHARE
rsync -av rsync://$ip/share ./ 
```