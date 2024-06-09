# Docker-php83-sqlsrv
docker desktop, run on for window os.

# Condition
Prerequisites : successfully installed docker desktop on windows os : https://www.docker.com/products/docker-desktop/

# Pull
docker pull tudonghoa/php_sqlsrv

# Install
docker run -d --name myweb --privileged --restart=always -v E:\Docker\myweb:/app -p 80:80 tudonghoa/php_sqlsrv
