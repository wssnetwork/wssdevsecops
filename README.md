# WssDevSecOps
WSS DevSecOps In Action
## Tools To Setup
1. If using virtualbox, enable 2 network adapters.
   1. Adapter 1, configure as NAT.
   2. Adapter 2, configure as Host-only.
   3. For every vbox image, if using ubuntu as base use netplan to configure static IP.
        ```
        network:
            ethernets:
                enp0s3:
                    dhcp4: true
                enp0s8:
                    addresses:
                        - 192.168.x.x/24 # the static ip
            version: 2
        ```
    4. Once the file save and exit, run `$ sudo netplan apply` to make the configuration effective.
2. The objective is to have every solution setup in Virtual Box.
   ![vbox sample](img/vbox-sample.jpg)
3. To easier connect per each instance, create entry in hosts file for each instance.
   1. In Windows
      1. Run cmd as administrator
      2. Go to directory as image below and run `notepad hosts`. Add suggested hosts and save.
        ![set hosts file in windows](img/set-hosts-file-in-windows.jpg)
   2. In Linux
4. Gitlab-ce as code repository and ci pipelines
   1. Setup gitlab-ce
      1. Link to refer https://computingforgeeks.com/how-to-install-gitlab-ce-on-ubuntu-linux/
      2. Login gitlab-ce
        1. To get gitlab-ce initial password, from the shell terminal in gitlab-ce machine:
            ```
            $ grep 'Password:' /etc/gitlab/initial_root_password
            ```
        2. Configure external_url.
            ```
            $ vi /etc/gitlab/gitlab.rb

            # --> press i
            # at line 32, change external_url 'http://gitlab-ce/'
            # --> press Esc > shift+: > wq > Enter

            $ gitlab-ctl reconfigure
            $ exit
            ```
        3. Access Gitlab at `http://gitlab-ce` (if already set in hosts file) or go to as IP address set earlier `http://<ip-addr>`
            ```
            username: root
            password: <output-from-command-at-step-1>
            ```
   2. Setup gitlab-runner 
      1. Link to refer https://docs.gitlab.com/runner/install/
      2. Once install, to register in `http://gitlab-ce`, at dvja project go to `Settings > CI/CD > Runners`. Take note on registration token.
      3. At host which gitlab-runner already installed, run command to register gitlab-runner.
        ```
        $ gitlab-runner register --url http://gitlab-ce/ --registration-token <token-from-step-2>
        ```
   3. Fill up the details. Just HIT ENTER to follow default details. Please make sure to choose `docker` as executor. Some sample below may different with yours.
        ```
        Enter the GitLab instance URL (for example, https://gitlab.com/):
        [http://gitlab-ce/]: HIT ENTER
        Enter the registration token:
        [<token>]: HIT ENTER
        Enter a description for the runner:
        [e5c5d3c3ef88]: dvja
        Enter tags for the runner (comma-separated):
        dvja
        Enter optional maintenance note for the runner:

        Registering runner... succeeded                     runner=GR1348941Ezg4zQn6
        Enter an executor: docker, docker-ssh, docker-ssh+machine, kubernetes, custom, parallels, shell, ssh, virtualbox, docker+machine:
        docker
        Enter the default Docker image (for example, ruby:2.7):
        alpine:latest
        Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
        
        Configuration (with the authentication token) was saved in "/etc/gitlab-runner/config.toml"
        ```
   4. Verify configuration at gitlab-ce.
        ![runner-1](img/runner-1.jpg)
   5. Click icon pencil at runner. Checked `Run untagged jobs` and verify config as below.
        ![runner-2](img/runner-2.jpg)
   6.  Edit config.toml in gitlab-runner container.
        ```
        $ vi /etc/gitlab-runner/config.toml
        
        # --> press i
        # change `privileged = false` to `privileged = true`
        # add extra_hosts = ["gitlab-ce:172.18.0.3"] --> below "volumes.." | IP refer to IP in docker network
        # --> press Esc > shift+: > wq > Enter

        $ exit
        ```





5.  Sonarqube as SAST tool
   7. Link to refer https://medium.com/@HoussemDellai/setup-sonarqube-in-a-docker-container-3c3908b624df
   8. Requirement to run sonarqube https://docs.sonarqube.org/latest/requirements/requirements/#header-4.
        Add this value in /etc/sysctl.conf
        ```
        vm.max_map_count=524288
        fs.file-max=131072
        ```
        And run this to get the value effective
        ```
        $ sudo sysctl -p /etc/sysctl.conf
        ```
   9. Access Sonarqube at `http://sonarqube:9000`. Once login with credential below will require change to new password.
        ```
        username: admin
        password: admin
        ```
6.  Dependency-check as SCA tool (integrate with Sonarqube)
   10. First enable Dependency-Check plugin in Sonarqube at Administration > Marketplace. Search the plugins, install and reboot the server
   11. Link to refer to integrate plugin in scanning https://github.com/dependency-check/dependency-check-sonar-plugin
   12. For DVJA, integrate plugin with maven https://jeremylong.github.io/DependencyCheck/dependency-check-maven/
7.  DVJA as vulnerable app and staging server
   13. Link to refer https://github.com/appsecco/dvja
   14. Access DVJA at `http://dvja:8080`. No user, will need to register if want to play DVJA.
8.  DefectDojo as vulnerability management tool
   15. Link to refer https://github.com/DefectDojo/django-DefectDojo#quick-start
## To Start DevSecOps
1. If in Windows, use WSL2 Ubuntu and link with Docker-Desktop. Easy to manage with VS Code connect to WSL2 (https://code.visualstudio.com/docs/remote/wsl).
2. Clone this repository in WSL2 Ubuntu, set permission as read and enter the directory.
    ```
    $ sudo git clone https://github.com/wssnetwork/wssdevsecops
    $ sudo chown -R <user> wssdevsecops/dvja
    $ cd wssdevsecops/dvja
    ```
### To use DVJA codebase and push to local Gitlab
1. Create blank project in gitlab-ce `http://gitlab-ce` as below. 
    ![gitlab blank project](img/gitlab-blank-project.jpg)
3. Once project created, run git init in dvja directory. (can follow step as suggest in gitlab-ce page)
    ```
    $ git init --initial-branch=main
    $ git config --global user.email "youremail@yourdomain.com"
    $ git config --global user.name "your username"
    $ git config --global --add safe.directory /wssdevsecops/dvja
    $ git remote add origin http://gitlab-ce/root/dvja.git
    $ git add .
    $ git commit -m "initial commit"
    $ git push -u origin main
    ```
4. Verify project uploaded in gitlab-ce.
# Things To Do
