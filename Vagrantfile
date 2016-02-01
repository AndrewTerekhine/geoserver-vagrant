# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::configure("2") do |config|
  config.vm.hostname = "hostname.com"

  if Vagrant.has_plugin?("vagrant-cachier")
    # Configure cached packages to be shared between instances of the same base box.
    # More info on http://fgrehm.viewdocs.io/vagrant-cachier/usage
    config.cache.scope = :box

    # OPTIONAL: If you are using VirtualBox, you might want to use that to enable
    # NFS for shared folders. This is also very useful for vagrant-libvirt if you
    # want bi-directional sync
    config.cache.synced_folder_opts = {
      type: :nfs,
      # The nolock option can be useful for an NFSv3 client that wants to avoid the
      # NLM sideband protocol. Without this option, apt-get might hang if it tries
      # to lock files needed for /var/cache/* operations. All of this can be avoided
      # by using NFSv4 everywhere. Please note that the tcp option is not the default.
      mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
    }
    # For more information please check http://docs.vagrantup.com/v2/synced-folders/basic_usage.html
  end

  # VirtualBox configuration for Digital Ocean
  config.vm.provider :digital_ocean do |provider, override|
    override.ssh.private_key_path = '~/.ssh/id_rsa'
    override.vm.box = 'digital_ocean'
    override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"

    provider.ssh_key_name = "Vagrant"
    provider.token = '' # Place your Digital Ocean token here
    provider.image = 'ubuntu-14-04-x64'
    provider.private_networking = true
    provider.ipv6 = true
    provider.region = 'nyc2'
    provider.size = '1gb'
    provider.backups_enabled = false
  end

  config.vm.network :forwarded_port, guest: 8080, host: 8080, auto_correct: true
  config.vm.network "private_network", ip: "192.168.50.4"

  #config.vm.synced_folder "data/", "/data", owner: "tomcat6", group: "tomcat6", type: "nfs"
  config.vm.synced_folder "data/", "/data", 
    :nfs => true,
    :linux__nfs_options => ['rw','no_subtree_check','no_root_squash','async']
  # Share an additional folder to the guest VM. The first argument is
  # an identifier, the second is the path on the guest to mount the
  # folder, and the third is the path on the host to the actual folder.
  # config.vm.share_folder "v-data", "/vagrant_data", "../data"

 # Provisioning with chef solo
  config.vm.provision :chef_solo do |chef|
    chef.data_bags_path = "data_bags"
    chef.log_level = :debug
    chef.add_recipe "apt"
    chef.add_recipe "java"
    chef.add_recipe "tomcat"
    chef.add_recipe "tomcat::users"
    chef.add_recipe "geoserver::install_msfonts"
    chef.add_recipe "geoserver"
    chef.add_recipe "geoserver::add_wps"
    chef.add_recipe "geoserver::install_ordnancesurvey_fonts"

    chef.json = {
      :geoserver => {:version => '2.8.1',
        :data_dir => '/data/geoserver' },
      :install_repo => {:repos => [
        "deb http://us.archive.ubuntu.com/ubuntu/ trusty multiverse",
        "deb-src http://us.archive.ubuntu.com/ubuntu/ trusty multiverse",
        "deb http://us.archive.ubuntu.com/ubuntu/ trusty-updates multiverse",
        "deb-src http://us.archive.ubuntu.com/ubuntu/ trusty-updates multiverse"
      ]},
      :java => {
        :jdk_version => "8",
      },
#      :java => {
#        :install_flavor => "oracle",
#        :jdk_version => "8",
#        :oracle => {
#          :accept_oracle_download_terms => true
#        }
#      },
      :tomcat => {
            'catalina_options' => "-DGEOSERVER_DATA_DIR=/data/geoserver",
            'java_options' => "${JAVA_OPTS} -Djava.awt.headless=true -Xmx1024m -XX:NewSize=256m -XX:MaxNewSize=256m -XX:PermSize=512m -XX:MaxPermSize=512m -XX:CompileCommand=exclude,net/sf/saxon/event/ReceivingContentHandler.startElement",
            :keystore_password => "ianian"
      }
    }
  end

  config.ssh.forward_agent = true
  config.ssh.forward_x11 = true
end
