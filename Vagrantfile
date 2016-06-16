# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "dummy"
  # Download the aws "dummy" image if it isn't already isntalled
  config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider :aws do |aws, override|
    aws.access_key_id = ENV['AWS_KEY']
    aws.secret_access_key = ENV['AWS_SECRET']
    aws.keypair_name = ENV['AWS_KEYNAME'] 
    aws.region = "us-east-1"
    aws.ami = "ami-a4827dc9"
    aws.subnet_id = ""
    aws.instance_type = "t2.medium"
    aws.security_groups = ""

    aws.tags = {
      'Name' => "Vagrant workstation"
    }
    aws.associate_public_ip = "true"
    override.ssh.username = "ec2-user"
    override.ssh.private_key_path = ""

    config.ssh.forward_agent = true
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provision/playbook.yml"
    # Allow ansible to provision all machines
    ansible.limit = "all"
    # Enable agent forwarding for ansible provisioner
    ansible.extra_vars = { ansible_ssh_user: 'ec2-user', 
                           ansible_ssh_private_key_file: '',
                           ansible_connection: 'ssh',
                           ansible_ssh_args: '-o ForwardAgent=yes'}
                           
  end
end
