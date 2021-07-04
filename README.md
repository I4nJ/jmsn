# jmsn

Use of [LXD](https://linuxcontainers.org/lxd/getting-started-cli/) to run my services and playground stuff

### Development environment 

This all needs a linux environment. I like ubuntu. Thus the first setup will be to get an ubnutu environment running.


#### Windows 10
This is assuming windows 10 Pro. In the near future WSL will probably be all you need but at the time of writing LXD was not easy to install 
on WSL because it requires to be installed via snap and seemed to require snap's daemon runnng. Snap did not work because WSL does not have 
systemd. Another option is to use Virtualbox if Windows 10 Pro is not an option and only windows home is available. 


__Option 1__: [multipass](https://multipass.run)   

Use multipass run a ubuntu virtual machine with hyper-v (preferably) or Virtualbox. LXD containers will be hosted in the virtual machine.

> Important: If you HAVE to use virtualbox, read the multipass documents carefully, virtualbox needs to be run as the system user for use with multipass. Use `PsExec.exe -s` to do that.

With a cloud-init.yaml file something like this....
```yaml
users:
  - name: ubuntu
    ssh_authorized_keys:
      - <SSH_PUBLIC_KEY> #will need to generate ssh keys before hand
```

... use multipass to lauch an new virtaul machine
```bash
multipass launch -c 4 -d 20G -m 8G -n jmsn --cloud-init .\cloud-init.yaml -vvv
```

This will setup a local virtual ubuntu server that can be ssh'd into. Use the "Remote - SSH" vscode extension to connect to it or your favorite terminal program to ssh in.

__Option 2__:   
vagrant with virtualbox is another option to look into if multipass doesn't work out.

__Option 3__:
WSL in the near future will have snap support and LXD will be easier to get working in WSL.

#### Environment setup

Now that we are in the comfortable embrace of a linux terminal, here are the requirements to setup the development environment

```bash
# if python 3 not already installed
sudo apt install python3   

# pip3, because ansible in ubuntu repositories is too old
# if not already installed
sudo apt install python3-pip

#make sure pip is updated.
sudo pip3 install --upgrade pip 

#install ansible into user space, do not need to install into system
#note that it's pip not pip3 now since we installed the latest in the command above
pip install --user ansible    

# clone this repository. 
git clone git@github.com:I4nJ/jmsn.git
cd jmsn

#Use ansible to run the playbook
#that sets up the local virtualmachine we are in as the development 
#environment with LXC and it's containers
cd ansible
ansible-playbook update_metal.yml -i development.ini
```


