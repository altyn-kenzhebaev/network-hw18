# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:inetRouter => {
        :box_name => "centos/7",
        :ans_addr => '192.168.50.100',
        :ans_adp => 3,
        :nets => {
                  :net2 => {
                    :ip => '192.168.255.1', 
                    :adapter => 2, 
                    :netmask => "255.255.255.252", 
                    :virtualbox__intnet => "router-net-central"
                  },
                 }
  },

  :centralRouter => {
        :box_name => "centos/7",
        :ans_addr => '192.168.50.150',
        :ans_adp => 8,
        :nets => {
                  :net2 => {
                    :ip => '192.168.255.2', 
                    :adapter => 2, 
                    :netmask => "255.255.255.252", 
                    :virtualbox__intnet => "router-net-central"
                  },
                  :net3 => {
                    :ip => '192.168.0.1', 
                    :adapter => 3,
                    :netmask => "255.255.255.240",
                    :virtualbox__intnet => "dir-net"
                  },
                  :net4 => {
                    :ip => '192.168.0.33', 
                    :adapter => 4,
                    :netmask => "255.255.255.240",
                    :virtualbox__intnet => "hw-net"
                  },
                  :net5 => {
                    :ip => '192.168.0.65',
                    :adapter => 5, 
                    :netmask => "255.255.255.192",
                    :virtualbox__intnet => "wifi-net"
                  },
                  :net6 => {
                    :ip => '192.168.255.5', 
                    :adapter => 6, 
                    :netmask => "255.255.255.252", 
                    :virtualbox__intnet => "office1-central"
                  },
                  :net7 => {
                    :ip => '192.168.255.9', 
                    :adapter => 7, 
                    :netmask => "255.255.255.252", 
                    :virtualbox__intnet => "office2-central"
                  },
                }
  },
  
  :centralServer => {
        :box_name => "centos/7",
        :ans_addr => '192.168.50.151',
        :ans_adp => 3,
        :nets => {
                  :net2 => {
                    :ip => '192.168.0.2',
                    :adapter => 2,
                    :netmask => "255.255.255.240",
                    :virtualbox__intnet => "dir-net"
                  },
                }
  },

  :office1Router => {
    :box_name => "centos/7",
    :ans_addr => '192.168.50.120',
    :ans_adp => 7,
    :nets => {
              :net2 => {
                :ip => '192.168.255.6', 
                :adapter => 2, 
                :netmask => "255.255.255.252", 
                :virtualbox__intnet => "office1-central"
              },
              :net3 => {
                :ip => '192.168.2.1', 
                :adapter => 3,
                :netmask => "255.255.255.192",
                :virtualbox__intnet => "dev-office1-net"
              },
              :net4 => {
                :ip => '192.168.2.65', 
                :adapter => 4,
                :netmask => "255.255.255.192",
                :virtualbox__intnet => "test-office1-net"
              },
              :net5 => {
                :ip => '192.168.2.129',
                :adapter => 5, 
                :netmask => "255.255.255.192",
                :virtualbox__intnet => "mngr-office1-net"
              },
              :net6 => {
                :ip => '192.168.2.193', 
                :adapter => 6, 
                :netmask => "255.255.255.252", 
                :virtualbox__intnet => "hw-office1-net"
              },
            }
  },

  :office1Server => {
    :box_name => "centos/7",
    :ans_addr => '192.168.50.121',
    :ans_adp => 3,
    :nets => {
              :net2 => {
                :ip => '192.168.2.130',
                :adapter => 2, 
                :netmask => "255.255.255.192",
                :virtualbox__intnet => "mngr-office1-net"
              },
            }
  },

  :office2Router => {
    :box_name => "centos/7",
    :ans_addr => '192.168.50.220',
    :ans_adp => 6,
    :nets => {
              :net2 => {
                :ip => '192.168.255.10', 
                :adapter => 2, 
                :netmask => "255.255.255.252", 
                :virtualbox__intnet => "office2-central"
              },
              :net3 => {
                :ip => '192.168.1.1', 
                :adapter => 3,
                :netmask => "255.255.255.128",
                :virtualbox__intnet => "dev-office2-net"
              },
              :net4 => {
                :ip => '192.168.1.129', 
                :adapter => 4,
                :netmask => "255.255.255.192",
                :virtualbox__intnet => "test-office2-net"
              },
              :net5 => {
                :ip => '192.168.1.193', 
                :adapter => 5, 
                :netmask => "255.255.255.252", 
                :virtualbox__intnet => "hw-office2-net"
              },
            }
  },

  :office2Server => {
    :box_name => "centos/7",
    :ans_addr => '192.168.50.221',
    :ans_adp => 3,
    :nets => {
              :net2 => {
                :ip => '192.168.1.130',
                :adapter => 2, 
                :netmask => "255.255.255.192",
                :virtualbox__intnet => "test-office2-net"
              },
            }
  },

}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s

        boxconfig[:nets].each do |netname, netconf|
          box.vm.network "private_network", 
          ip: netconf[:ip], 
          adapter: netconf[:adapter], 
          netmask: netconf[:netmask], 
          virtualbox__intnet: netconf[:virtualbox__intnet]
        end

        box.vm.network "private_network", adapter: boxconfig[:ans_adp], ip: boxconfig[:ans_addr]

        box.vm.provision "ansible" do |ansible|
          ansible.playbook = 'ansible/' + boxname.to_s + '.yml'
          ansible.compatibility_mode = "2.0"
        end

      end

  end
  
end
