# Crear directorio donde se almacenan los key
mkdir -p /var/jenkins_home/.ssh/

# Es para escanear y decir que la pagina github.com es segura 
ssh-keyscan -t ed25519 github.com >> /var/jenkins_home/.ssh/known_hosts