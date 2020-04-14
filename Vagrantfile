# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure('2') do |config|
  # FIXME: When upgrading to a future version of Ubuntu check if the workaround
  # near the top of bootstrap.sh is still needed. If not, please delete it.
  config.vm.box      = 'ubuntu/bionic64' # 18.04
  config.disksize.size = '30GB'
  config.vm.hostname = 'dev-box'

  config.vm.network :forwarded_port, guest: 5432, host: 5433
  config.vm.network :forwarded_port, guest: 8080, host: 8080
  config.vm.network :private_network, ip: "192.168.33.20"
  config.vm.synced_folder "/Users/mashhood-macosx/Desktop/projects/", "/projects/"

  # Install the plugin:
  # vagrant plugin install vagrant-listen-server
  if Vagrant.has_plugin? 'vagrant-listen-server'
    # The host will always be the same IP as the private network with the last
    # octet as .1
    config.listen_server.ip = '192.168.33.1'
    config.listen_server.port = 4000
    config.listen_server.folders = '/Users/mashhood-macosx/Desktop/projects/'
    config.listen_server.pid_file = '/tmp/servername.listen.pid'
  end
  # Because sleep states of both the host and guest machines can mess with long connections in unexpected ways, you can use the command to control the listen server.
  # vagrant listen stop
  # vagrant listen start
  # vagrant listen status

  config.vm.provision :bootstrap, type: :shell, path: 'bootstrap.sh', keep_color: true, preserve_order: true
  config.vm.provision :shift_config, type: :shell, path: 'config.sh', keep_color: true, preserve_order: true

  config.ssh.username = 'ubuntu'
  config.ssh.password = "thepassword"

  config.vm.provider 'virtualbox' do |v|
    v.memory = ENV.fetch('RAILS_DEV_BOX_RAM', 2048).to_i
    v.cpus   = ENV.fetch('RAILS_DEV_BOX_CPUS', 2).to_i
  end
end
