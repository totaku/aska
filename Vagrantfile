# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "totaku/aska"
  config.vm.network "private_network", ip: "192.168.33.77"
  config.vm.hostname = "aska"
  config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]

  config.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get upgrade -y

        ## Only thing you probably really care about is right here
        DOMAINS=()

        ## Loop through all sites
        for ((i=0; i < ${#DOMAINS[@]}; i++)); do

            ## Current Domain
            DOMAIN=${DOMAINS[$i]}

            echo "Creating directory for $DOMAIN..."
            mkdir -p /var/www/$DOMAIN

            echo "Creating vhost config for $DOMAIN..."
            sudo cp /var/www/drupal /etc/nginx/conf.d/$DOMAIN.conf

            echo "Updating vhost config for $DOMAIN..."
            sudo sed -i s,aska.local,$DOMAIN,g /etc/nginx/conf.d/$DOMAIN.conf
            sudo sed -i s,/var/www/html,/var/www/$DOMAIN,g /etc/nginx/conf.d/$DOMAIN.conf

            echo "So let's restart nginx..."
            sudo systemctl reload nginx

        done

    SHELL

end
