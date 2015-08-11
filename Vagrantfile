# -*- mode: ruby -*-
# vi: set ft=ruby :

$addContainer=<<EOF
    docker run --name some-redis -d redis
EOF

$addPrometheus=<<EOF
  docker run -v /tmp/prom:/tmp/prom -e DATABASE_URL=sqlite3:/tmp/prom/file.sqlite3 prom/promdash ./bin/rake db:migrate

  docker run -d -p 3000:3000 -v /tmp/prom:/tmp/prom -e DATABASE_URL=sqlite3:/tmp/prom/file.sqlite3 prom/promdash

  docker run -d -p 9093:9093 -v /vagrant/alertmanager.conf:/alertmanager.conf prom/alertmanager -config.file=/alertmanager.conf

  docker run -d --name prometheus -p 9090:9090 -v /vagrant/prometheus.conf:/prometheus.conf --link cadvisor:cadviser prom/prometheus -config.file=/prometheus.conf -alertmanager.url=http://localhost:9093
EOF

Vagrant.configure(2) do |config|
  
  config.vm.define "dockerOne" do |dockerOne|
    dockerOne.vm.box = "williamyeh/ubuntu-trusty64-docker"
    dockerOne.vm.network "private_network", ip: "192.168.33.10"
    dockerOne.vm.provision "shell", inline: $addContainer
  end

  config.vm.define "dockerTwo" do |dockerTwo|
    dockerTwo.vm.box = "williamyeh/ubuntu-trusty64-docker"
    dockerTwo.vm.network "private_network", ip: "192.168.33.11"
    dockerTwo.vm.provision "shell", inline: $addContainer
  end
  
  config.vm.define "prometheus" do |prometheus|
    prometheus.vm.box = "williamyeh/ubuntu-trusty64-docker"
    prometheus.vm.network "forwarded_port", guest: 9090, host: 9090
    prometheus.vm.network "forwarded_port", guest: 3000, host: 3000
    prometheus.vm.network "forwarded_port", guest: 9093, host: 9093
    prometheus.vm.network "private_network", ip: "192.168.33.12"
    prometheus.vm.provision "shell", inline: $addPrometheus
  end
end
