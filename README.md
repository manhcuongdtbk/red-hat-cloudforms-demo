# Red Hat CloudForms Install Demo

An updated and fully functional Red Hat CloudForms Demo created for Cloud Computing researching purpose.

## Prerequisites

1. Hardwares
   - Strongly recommend using SSD. If not, your computer may freeze and never pass creating Virtualization Machine step.
   - At least 8GB RAM. If not, your computer will freeze and / or auto restart at some points.
2. Operating Systems
   - Recommended: Mac OS X (>= 10.13.6, works best on 10.13.6)
   - Other OSs: Red Hat Enterprise Linux (>= 7), Centos (>= 7)
3. Personality: If you are not willing to learn new things, too lazy to ask me (@b-cuong) first then Google the answer if I can’t give you the proper answer, and patient when you have to wait for the machine runs and do its magic to setup what you need to have all the needed things for this project, you should quit NOW.

Welcome to our journey in discovering Red Hat Cloudforms. I will always be your wingman throughout the journey. Have fun!

## Install on Mac OS X

1. Install Homebrew:

   ```bash
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```

2. Install Docker Machine Driver XHYVE:

   ```bash
   brew install docker-machine-driver-xhyve
   sudo chown root:wheel $(brew --prefix)/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
   sudo chmod u+s $(brew --prefix)/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
   ```

3. Install Minishift:

   - Create Red Hat Account: https://www.redhat.com/en
   - Download [minishift](https://developers.redhat.com/products/cdk/download)
   - Copy `minishift` to correct location

   ```bash
   mkdir -p ~/bin
   cp ~/Downloads/#{DOWNLOADED_FILE_NAME} ~/bin/minishift
   chmod +x ~/bin/minishift
   ```

   - Add `minishift` to `PATH`

   ```bash
   echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
   export PATH=$PATH:$HOME/bin
   ```

   - Setup `minishift cdk`

   ```bash
   minishift setup-cdk
   ```

   - Start `minishift`

   ```bash
   minishift start
   ```

   - Enter your Red Hat username and password
   - Errors that you may encouter and how to fix them:

     - XHYVE driver error:

       - _Error starting the VM: Error creating new host: json: cannot unmarshal bool into Go struct field Driver.Virtio9p of type []string_

         ```bash
         brew unlink docker-machine-driver-xhyve
         brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/7310c563d662ddbe094f46f9600cad30ad3551a6/Formula/docker-machine-driver-xhyve.rb
         sudo chown root:wheel /usr/local/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
         sudo chmod u+s /usr/local/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
         ```

       - _json: cannot unmarshal bool into Go struct field Driver.Virtio9p of type []string_

         ```bash
         minishift delete
         brew uninstall docker-machine-driver-xhyve --force
         rm -rf /usr/local/bin/docker-machine-driver-xhyve
         brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/7310c563d662ddbe094f46f9600cad30ad3551a6/Formula/docker-machine-driver-xhyve.rb
         sudo chown root:wheel /usr/local/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
         sudo chmod u+s /usr/local/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
         brew cask install --force minishift
         ```

   - Save `minishift` infos for later use:

     ```bash
     -- Starting profile 'minishift'
     ...
     -- Minishift VM will be configured with ...
        Memory:    4 GB
        vCPUs :    2
        Disk size: 20 GB
     -- Starting Minishift VM .......................... OK
     -- Registering machine using subscription-manager
     ...
        Registration in progress ..................... OK [42s]
     ...
     Extracting
     Image pull complete
     OpenShift server started.

     The server is accessible via web console at:
        https://192.168.42.60:8443

     You are logged in as:
        User:     developer
        Password:

     To login as administrator:
        oc login -u system:admin
     ...
     ```

   - Make `oc` works along with `minishift`

     ```bash
     eval $(minishift oc-env)
     ```

4. Add host config for `cloudforms`

   - Open host file

     ```bash
     sudo nano /etc/hosts
     ```

   - Paste to the end of hosts file, change IP address to your `minishift` created IP address

     ```bash
     # add host for Red Hat Cloudforms Demo
     192.168.99.100 cloudforms-cloudforms.192.168.99.100.nip.io
     ```

5. Init `cloudforms`

   ```bash
   # The installation needs to be pointed to a running version
   # of OpenShift, so pass an IP address such as:
   #
   ./init.sh 192.168.99.100  # example for OCP.
   ```

6. After successfully init `cloudforms`, open web browser with your IP address. For example: <https://192.168.42.60:8443/console>
