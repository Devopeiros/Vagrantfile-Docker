Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64" #Escolha a "box/imagem" que você deseja usar
  config.vm.define "docker-node-01" do |docker|
  config.vm.box_download_insecure=true #Bypass nas imagens não consideradas "oficiais"  
  config.vm.provision "shell", inline: <<-SHELL
    # Atualiza a lista de pacotes
    sudo apt-get update

    # Instala pacotes essenciais
    sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common

    # Adiciona a chave GPG oficial do Docker
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

    # Adiciona o repositório do Docker
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

    # Atualiza a lista de pacotes novamente
    sudo apt-get update

    # Instala o Docker
    sudo apt-get install -y docker-ce

    # Adiciona o usuário ao grupo do Docker para não precisar usar sudo
    sudo usermod -aG docker vagrant
  SHELL
  end
end  