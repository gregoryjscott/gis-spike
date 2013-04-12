# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|

  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.network :hostonly, "192.168.10.10"

  # Forward a port from the guest to the host, which allows for outside
  # computers to access the VM, whereas host only networking does not.
  # config.vm.forward_port 80, 8080

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks"]
    chef.add_recipe "apt"
    chef.add_recipe "build-essential"
    chef.add_recipe "postgresql"
    chef.add_recipe "postgresql::client"
    chef.add_recipe "postgresql::libpq"
    chef.add_recipe "postgresql::server"
    chef.add_recipe "gis"

    chef.json = {
      "postgresql"=> {
        "version"=> "9.1",
        listen_addresses: "*",
        pg_hba: [
          { type: "host", db: "all", user: "all", addr: "192.168.10.0/24", method: "md5"}
        ],
        users: [
          {
            username: "superman",
            password: "isdead",
            superuser: true,
            createdb: true,
            login: true
          },
          {
            username: "anchorage_mapped",
            password: "trololol",
            superuser: false,
            createdb: true,
            login: true
          }
        ],
      },

      "gis" => {
        "ubuntugis" => {
          "repo" => "unstable",
          "packages" => ['gdal-bin', 'python-gdal', 'libxml2-dev', 'libgeos-dev', 'proj', 'postgis', 'postgresql-9.1-postgis']
        }
      }
    }
  end
end