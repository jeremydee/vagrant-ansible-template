# all the variables you need to set are listed here
vm_provider_name        = "virtualbox"      # Required - virtualbox, parallels, etc.
vm_box_name             = "ubuntu/trusty64" # Required - Change this to any box you want
vm_box_memory           = "1024"            # Required
box_name                = "test3"           # Required - An arbitrary name to call your local VM
local_host_name         = "test3.vm"        # Required - Hosts file entry
local_ip                = "192.168.33.102"  # Required
test_host_name          = ""
test_ip                 = ""
prod_host_name          = ""
prod_ip                 = ""
digital_ocean_token     = "" # V2 API
mysql_db_names          = ["database1"]     # Required - BUG - accepts single element array only
mysql_root_pass         = "rootpass"        # Required
mysql_app_user          = "appuser"         # Required
mysql_app_pass          = "apppass"         # Required
my_public_key           = ""                # Optional contents of ~/.ssh/id_rsa.pub

# previous comment is not really true
# you will likely want to set the box type/image types below too
# you may also want to control the Digital Ocean region or VM sizes too
Vagrant.configure("2") do |config|

    config.vm.provider vm_provider_name do |v|
        v.name = box_name
        v.memory = vm_box_memory
    end

    config.vm.define "local", primary: true do |local|

        local.vm.box = vm_box_name
        local.vm.hostname = local_host_name

        local.vm.network :private_network, ip: local_ip

        local.vm.synced_folder "./", "/vagrant", owner: "www-data", group: "www-data"

        config.vm.provision "ansible" do |ansible|
            ansible.extra_vars = {
                server_name: "192.168.33.102",
                application_env: "development",
                host_name: local_host_name,
                mysql_root_pass: mysql_root_pass,
                mysql_db_names: mysql_db_names,
                mysql_app_user: mysql_app_user,
                mysql_app_pass: mysql_app_pass,
                my_public_key: my_public_key
            }
            ansible.playbook = "ansible/playbook.yml"
        end
    end

#    config.vm.define "test" do |test|
#
#        test.vm.box = "digital_ocean"
#        test.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"
#        test.vm.hostname = test_host_name
#
#        # do not allow vagrant to sync files
#        # use some other mechanism (like git) to deploy files
#        test.vm.synced_folder "./", "/vagrant", disabled: true
#
#        test.vm.provider :digital_ocean do |provider, override|
#            override.ssh.private_key_path = "~/.ssh/id_rsa"
#            provider.token = digital_ocean_token
#            provider.image = "ubuntu-14-04-x64"
#            provider.region = "sfo1"
#            provider.size = "1GB"
#        end
#
#        config.vm.provision "ansible" do |ansible|
#            ansible.extra_vars = {
#                server_name: test_ip,
#                application_env: "testing",
#                host_name: test_host_name
#            }
#            ansible.playbook = "ansible/playbook.yml"
#        end
#    end
#
#    config.vm.define "prod" do |prod|
#
#        prod.vm.box = "digital_ocean"
#        prod.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"
#        prod.vm.hostname = prod_host_name
#
#        # do not allow vagrant to sync files
#        # use some other mechanism (like git) to deploy files
#        prod.vm.synced_folder "./", "/vagrant", disabled: true
#
#        prod.vm.provider :digital_ocean do |provider, override|
#            override.ssh.private_key_path = "~/.ssh/id_rsa"
#            provider.token = digital_ocean_token
#            provider.image = "ubuntu-14-04-x64"
#            provider.region = "nyc3"
#            provider.size = "1GB"
#        end
#
#        config.vm.provision "ansible" do |ansible|
#            ansible.extra_vars = {
#                server_name: prod_ip,
#                application_env: "production",
#                host_name: prod_host_name
#            }
#            ansible.playbook = "ansible/playbook.yml"
#        end
#    end
end
