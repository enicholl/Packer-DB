ENV["GEM_PATH"] = nil
ENV["GEM_HOME"] = nil
required_plugins = %w( vagrant-hostsupdater vagrant-berkshelf )
required_plugins.each do |plugin|
  exec "vagrant plugin install #{plugin};vagrant #{ARGV.join(" ")}" unless Vagrant.has_plugin? plugin || ARGV[0] == 'plugin'
end
Vagrant.configure("2") do |config|
    config.omnibus.chef_version = '14.12.9'
    config.vm.define "app" do |app|
      app.vm.box = "ubuntu/xenial64"
      app.vm.network "private_network", ip: "192.168.10.200"
      app.hostsupdater.aliases = ["development1.local"]
      app.vm.synced_folder "app", "/app"
      app.vm.provision "chef_solo" do |chef|
        chef.add_recipe "node::default"
      end
    end
    config.vm.define "db" do |db|
      db.vm.box = "ubuntu/xenial64"
      db.vm.network "private_network", ip: "192.168.10.250"
      db.hostsupdater.aliases = ["database1.local"]
      db.vm.provision "chef_solo" do |chef|
        chef.add_recipe "mongo::default"
      end



    end




end
