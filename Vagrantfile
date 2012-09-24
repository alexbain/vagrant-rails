# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|

  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.forward_port 80, 8080

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks"]

    chef.add_recipe "apt"
    chef.add_recipe "build-essential"

    chef.add_recipe "ruby_build"
    chef.add_recipe "rbenv::system"
    chef.add_recipe "rbenv::vagrant"

    chef.add_recipe "nginx"
    chef.add_recipe "unicorn"

    chef.add_recipe "mysql::server"
    chef.add_recipe "mysql::ruby"
    chef.add_recipe "initdb"

    chef.add_recipe "rails-lastmile"

    chef.json.merge!(
      :build_essential => {
        :compiletime => true
      },
      :rvm => {
        :default_ruby => 'ruby-1.9.2-p290',
        :gem_package => {
          'rvm_string' => 'ruby-1.9.2-p290'
        }
      },
      :mysql => {
        :server_root_password => 'root',
        :bind_address => '127.0.0.1'
      },
      :initdb => {
        :mysql => {
          :connection => {
            :username => "root",
            :password => "root",
            :host     => "localhost"
          },
          :databases => {
            "rails" => {
              :action => :create
            }
          },
          :users => {
            "dbuser" => {
              :action        => :grant,
              :database_name => "rails",
              :host          => "localhost",
              :password      => "password"
            }
          }
        }
      }
    )
  end

end
