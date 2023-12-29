# Docker_install

'''
sudo yum update

'''

### docker install process

1. install docker 
''' sudo yum install docker '''

2. start docker Demon
''' sudo systemctl start docker '''

    if you run docker ps whithout stsrtig Docker Demon it will give error 
    *Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?*
    -> Check the status of docker 
    ''' sudo systemctl status docker ''' will give the delow error

    docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: https://docs.docker.com

3. Add the ec2-user ($USEER) the preset user to te docker group

    * To Check the group entries ''' getent group '''
    * To check the user entries ''' genent passwd '''
    * To check user in part of whick groups ''' groups $USER ''' *Note: $USER user must be caps*
    * try now any docker cmd docker ps if example 
        stll getting any error like 
        Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
        check the own of /var/run/docker.sock to the _rootuser:DockerGroup_ which is _root:docker_ 
        by using *ls-la /var/run/docker.sock*

4. error: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json": dial unix /var/run/docker.sock: connect: permission denied

    1. Check Docker Socket Permissions:
       Verify the ownership and permissions of the Docker socket:

        ''' ls -l /var/run/docker.sock '''
        Ensure that the ownership is root:docker. If it's not, you can adjust it:
        sudo chown root:docker /var/run/docker.sock

    2. Make sure your user is in the docker group:
        ''' sudo usermod -aG docker $USER '''
        After adding your user to the docker group, you may need to log out and log back in for the changes to take effect.

    3. Restart Docker:
        Restart the Docker daemon after making any changes:
        ''' sudo systemctl restart docker '''

    4. Check Group Membership:
        Ensure that your user is a member of the docker group. You can check this by running:
        ''' groups '''
        Look for docker in the list of groups. If it's not there, you might need to log out and log back in to apply the group changes.

