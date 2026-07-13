
# master redise connection test edebilirsin 
nc -zv <10.0.0.10> 6379

protected-mode no = test ortaminda 
protected-mode yes = production ortaminda bunu kullanip redis.conf dosyasinda <requirepass> ile daha guvenli hale getirebilirsin 
bu secenek aktif ise <requirepass> slavelerde ve clientlarda bunu kullanmam gerekiyor baglanmak icin ... 


# persistence icin 
appendonly yes # bu aof saglar ve daha guncel verilere erismeni saglar 

# RDB (redis database backup) da acik kalabilir 
save 900 1 
save 300 10
save 60 10000 # bu degerleri kendin ayarlayabilirsin..

# Slave redis configuration  
replicaof  <masterip> <master_redis_port>
replicaof   10.0.0.10 6379

# Master rediste requirepass ayari yapildi ise slave rediste bu configi ayarlaman gerekiyor 
masterauth <master-password>

redis-cli INFO replication # replication statusu gormek icin 

tcp-backlog 500  # Bekleyen bağlantı kuyruğu İş yüküne göre ayarla

bind 0.0.0.0 Redis, üzerinde bulunan tüm ağ arayüzlerinde (network interfaces) dinleme yapar. (bu anlama geliyor)
bind 127.0.0.1 10.0.0.4 boyle yaparsan sadece subnet ve localhosttan baglanti saglanir publicten baglanti saglanmaz .. 
# localhost + private network

protected-mode yes # Redis'in yanlış yapılandırılması sonucu internete açık hale gelmesini engelleyen güvenlik mekanizmasıdır.


redis-cli -h 10.0.0.10 baska makinede redis-cli baglanabilirsin bu sekilde protectded-mode no ise 

redis-cli -a MyStrongPassword123! # authentication aktif ise bunu kullanirsin 
masterauth MyStrongPassword123! # replica redislerde bunlari yapilandirman gerekiyor ... 
sentinel auth-pass mymaster MyStrongPassword123! # sentinelde ayni sekilde bilmesi gerekiyor ... 


# Redis ve Sentinelin kaldirilmasi 

sudo systemctl stop redis-server
sudo systemctl stop redis-sentinel
sudo systemctl disable redis-server
sudo systemctl disable redis-sentinel
sudo apt purge -y redis-server redis-sentinel

# kalan dosyalari sil 
sudo rm -rf /etc/redis
sudo rm -rf /var/lib/redis
sudo rm -rf /var/log/redis
sudo rm -rf /run/redis

# Replicalar her zaman master ile ayni datalari tutar bunu sakin unutma ... 
Replica'nın amacı veri saklamak değil, master'ın güncel durumunu yansıtmaktır.
