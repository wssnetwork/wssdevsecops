# WssDevSecOps
Need two docker-compose yml files
1. docker-compose for set of tools
2. docker-compose for defectdojo

## To start
1. Requirement to run sonarqube in docker-desktop (Windows machine). Update `vm.max_map_count=262144`.
    ```
    $ wsl -d docker-desktop
    $ echo 'vm.max_map_count=262144' >> /etc/sysctl.conf
    ```
2. Get into kali linux via bash terminal or connect remotely via vscode (https://code.visualstudio.com/docs/remote/containers)
    ```
    $ docker exec -it dso_kali /bin/bash
    ```
3. Run update and upgrade, then install git
    ```
    $ apt update && apt upgrade
    $ apt install git
    ```

## To get local copy for local gitlab
To use gitlab, git clone again wssdevsecops file in kali and upload local gitlab