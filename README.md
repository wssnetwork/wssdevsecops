# WssDevSecOps
Need two docker-compose yml files
1. docker-compose for set of tools
2. docker-compose for defectdojo

## To start
1. If in Windows, use WSL2 Ubuntu and link with Docker-Desktop. Easy to manage with VS Code connect to WSL2 (https://code.visualstudio.com/docs/remote/wsl)
2. Requirement to run sonarqube https://docs.sonarqube.org/latest/requirements/requirements/#header-4
    Add this value in /etc/sysctl.conf
    ```
    vm.max_map_count=524288
    fs.file-max=131072
    ```
    And run this to get the value effective
    ```
    $ sudo sysctl -p /etc/sysctl.conf
    ```
3. Clone this repository in WSL2 Ubuntu, set permission as read and enter the directory
    ```
    $ sudo git clone https://github.com/wssnetwork/wssdevsecops
    $ sudo chown -R <user> wssdevsecops
    $ cd wssdevsecops
    ```
4. Run docker-compose

## To get local copy for local gitlab
To use gitlab, git clone again wssdevsecops file in kali and upload local gitlab