# WssDevSecOps
WSS DevSecOps In Action
## To start
1. If in Windows, use WSL2 Ubuntu and link with Docker-Desktop. Easy to manage with VS Code connect to WSL2 (https://code.visualstudio.com/docs/remote/wsl).
2. Requirement to run sonarqube https://docs.sonarqube.org/latest/requirements/requirements/#header-4.
    Add this value in /etc/sysctl.conf
    ```
    vm.max_map_count=524288
    fs.file-max=131072
    ```
    And run this to get the value effective
    ```
    $ sudo sysctl -p /etc/sysctl.conf
    ```
3. Clone this repository in WSL2 Ubuntu, set permission as read and enter the directory.
    ```
    $ sudo git clone https://github.com/wssnetwork/wssdevsecops
    $ sudo chown -R <user> wssdevsecops
    $ cd wssdevsecops
    ```
4. Run docker-compose.
    First time
    ```
    $ docker-compose up --build -d
    ```
    Next time
    ```
    $ docker-compose up -d
    ```
### Login gitlab-ce
5. To get gitlab-ce initial password.
    ```
    $ docker exec -it gitlab-ce grep 'Password:' /etc/gitlab/initial_root_password
    ```
6. Access Gitlab at `http://127.0.0.1:8080`.
    ```
    username: root
    password: <output-from-command-at-step-5>
    ```
### Login sonarqube
7. Access Sonarqube at `http://127.0.0.1:9000`. Once login with credential below will require change to new password.
    ```
    username: admin
    password: admin
    ```
### DVJA
8. Access DVJA at `http://127.0.0.1:8088`. No user, will need to register if want to play DVJA.
### To use DVJA codebase and push to local Gitlab
9. In wssdevsecops directory, copy dvja folder to home (to separate git config).
    ```
    $ sudo cp -r dvja /home
    ```
10. Create blank project in gitlab-ce `http://127.0.0.1:8080` as below. 
    ![gitlab blank project](img/gitlab-blank-project.jpg)
11. 
# Things To Do
Need two docker-compose yml files
1. docker-compose for set of tools
2. docker-compose for defectdojo