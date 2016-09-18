# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'erb'

# Vagrantfile to provision Internet Explorer debugging environments.
#
# Features:
# * Using host DNS resolver, proxy
# * Provision guest hostfile
Vagrant.configure(2) do |config|

  # Helper to strip indentation in heredocs.
  # Code bluntly copied from rails/strip.rb (https://goo.gl/vheGnz)
  class String
    def strip_heredoc
      gsub /^#{self[/\A\s*/]}/, ''
    end
  end

  # Get hostsfile template path
  hostsfile_template_path =
      ENV.fetch('DEBUG_IE_HOSTSFILE_TEMPLATE', 'hostsfile.erb')

  # Read template from file or use inline template
  if File.file?(hostsfile_template_path)
    hostsfile_template = File.read(hostsfile_template_path)
  else
    # host ip - 10.0.2.2 is assigned by virtual machine and always  points to
    # your host environment.
    hostsfile_template = <<-ERB.strip_heredoc
      <% host_ip = '10.0.2.2' -%>
      # Hosts
      <%= host_ip %> localhost
      # <%= host_ip %> example.com
    ERB
  end

  # Configure machines
  config.vm.define 'vista-ie7' do |config|
    config.vm.box = 'modernIE/vista-ie7'
  end

  config.vm.define 'w7-ie8' do |config|
    config.vm.box = 'modernIE/w7-ie8'
  end

  config.vm.define 'w7-ie10' do |config|
    config.vm.box = 'modernIE/w7-ie10'
  end

  config.vm.define 'w7-ie11' do |config|
    config.vm.box = 'modernIE/w7-ie11'
  end

  config.vm.define 'w8-ie10' do |config|
    config.vm.box = 'modernIE/w8-ie10'
  end

  config.vm.define 'w8.1-ie11' do |config|
    config.vm.box = 'modernIE/w8.1-ie11'
  end

  config.vm.define 'w10-edge' do |config|
    config.vm.box = 'modernIE/w10-edge'
  end

  # Adjust the boot timeout since windows is very slow
  config.vm.boot_timeout = 500

  # Forward rdp
  config.vm.network 'forwarded_port', guest: 3389, host: 3389, id: 'rdp',
                    auto_correct: true

  # Configure winrm (default modern.ie user/password)
  config.vm.communicator = 'winrm'
  config.winrm.username = 'IEUser'
  config.winrm.password = 'Passw0rd!'

  # Provision machine hostfile
  hostsfile = ERB.new(hostsfile_template, nil, '-').result()
                  .encode(:crlf_newline => true)
  config.vm.provision 'shell',
                      inline: "'#{hostsfile}' | Out-File -encoding ASCII \
                        C:/Windows/System32/drivers/etc/hosts"

  # Virtualbox settings
  config.vm.provider 'virtualbox' do |vb|
    vb.gui = true
    vb.customize ['modifyvm', :id, '--memory', '1024']
    vb.customize ['modifyvm', :id, '--vram', '128']
    vb.customize ['modifyvm', :id, '--cpus', '2']
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    vb.customize ['guestproperty', 'set', :id,
                  '/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold', 10000]
  end
end
