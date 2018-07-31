$provision_script = <<SCRIPT
echo "10.11.8.2     redis001.vagrant.local" >> /etc/hosts
echo "10.11.8.3     redis002.vagrant.local" >> /etc/hosts
echo "10.11.8.4     redis003.vagrant.local" >> /etc/hosts
sudo apt-get update && \
    sudo apt-get upgrade -y && \
    sudo apt-get install -y redis-server
SCRIPT

$master_config = <<SCRIPT
sudo sed -i 's/bind 127.0.0.1/# Bound to 0.0.0.0/' /etc/redis/redis.conf
sudo sed -i 's/tcp-keepalive 0/tcp-keepalive 60/' /etc/redis/redis.conf
sudo sed -i 's/appendonly no/appendonly yes/' /etc/redis/redis.conf
echo "requirepass s0m3th!ngr0tt3n?" | sudo tee -a /etc/redis/redis.conf
echo "maxmemory-policy noeviction" | sudo tee -a /etc/redis/redis.conf
sudo systemctl restart redis-server
SCRIPT

$slave_config = <<SCRIPT
sudo sed -i 's/bind 127.0.0.1/# Bound to 0.0.0.0/' /etc/redis/redis.conf
echo "requirepass s0m3th!ngr0tt3n?" | sudo tee -a /etc/redis/redis.conf
echo "slaveof redis001.vagrant.local 6379" | \
    sudo tee -a /etc/redis/redis.conf
echo "masterauth s0m3th!ngr0tt3n?" | sudo tee -a /etc/redis/redis.conf
sudo systemctl restart redis-server
SCRIPT

Vagrant.configure("2") do |config|
    config.vm.define "redis01" do |redis01|
        redis01.vm.box = "ubuntu/xenial64"
        redis01.vm.hostname = "redis001.vagrant.local"
        redis01.vm.network "private_network", ip: "10.11.8.2"
        redis01.vm.provision :shell, inline: $provision_script
        redis01.vm.provision :shell, inline: $master_config
    end
    config.vm.define "redis02" do |redis02|
        redis02.vm.box = "ubuntu/xenial64"
        redis02.vm.hostname = "redis002.vagrant.local"
        redis02.vm.network "private_network", ip: "10.11.8.3"
        redis02.vm.provision :shell, inline: $provision_script
        redis02.vm.provision :shell, inline: $slave_config
    end
    config.vm.define "redis03" do |redis03|
        redis03.vm.box = "ubuntu/xenial64"
        redis03.vm.hostname = "redis002.vagrant.local"
        redis03.vm.network "private_network", ip: "10.11.8.4"
        redis03.vm.provision :shell, inline: $provision_script
        redis03.vm.provision :shell, inline: $slave_config
    end
end