![devopeiros_logo](https://devopeiros.com.br/images/logo-darkmode_huf6206a142be6e7706be8cf1485b2d3cb_3243_320x0_resize_q90_h2_lanczos_3.webp) 


# Vagrantfile


Um `Vagrantfile` é um arquivo de configuração usado pelo Vagrant, uma ferramenta de código aberto para criação e gerenciamento de ambientes de desenvolvimento virtualizados. O Vagrantfile descreve as características da máquina virtual que você deseja criar, incluindo o sistema operacional, recursos, rede, provisionamento e muito mais.

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64" # Escolha a box que você deseja usar
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
```

## Explicando o Código

* `Vagrant.configure("2") do |config|`: Inicia a configuração do Vagrant usando a versão 2 da API.

* `config.vm.box = "ubuntu/jammy64"`: Define a box a ser usada para criar a máquina virtual. Nesse caso, está usando a box "ubuntu/jammy64", que provavelmente é uma imagem Ubuntu para a versão "Jammy" do sistema.

`config.vm.define "docker-node-01" do |docker|`: Define uma máquina virtual com o nome "docker-node-01" usando a configuração interna "docker".

`config.vm.box_download_insecure=true`: Habilita o download de boxes não oficiais, permitindo que você use uma box que não seja das fontes oficiais do Vagrant.

`config.vm.provision "shell", inline: <<-SHELL ... end`: Especifica uma provisão do tipo "shell" que será executada na máquina virtual após ela ser criada. O bloco inline: contém um script shell que será executado no ambiente da máquina virtual. Esse script realiza as seguintes ações:

* Atualiza a lista de pacotes.
* Instala pacotes essenciais (apt-transport-https, ca-certificates, curl, software-properties-common).
* Adiciona a chave GPG oficial do Docker.
* Adiciona o repositório do Docker.
* Atualiza a lista de pacotes novamente.
* Instala o Docker.
* Adiciona o usuário "vagrant" ao grupo "docker" para que não seja necessário usar sudo ao executar comandos Docker.

`end`: Encerra o bloco de configuração da máquina virtual "docker-node-01".
`end`: Encerra o bloco de configuração do Vagrantfile.

Esse **Vagrantfile** cria uma máquina virtual chamada *"docker-node-01"* baseada na box *"ubuntu/jammy64"*, instala o Docker e configura o usuário "vagrant" para executar comandos Docker sem a necessidade de usar sudo. Lembre-se de que esse é apenas um exemplo e pode ser ajustado de acordo com suas necessidades e preferências. Certifique-se de entender cada etapa antes de usar esse Vagrantfile em um ambiente de produção.


## Lista de comandos básicos do Vagrant na linha de comando (CLI), acompanhados de comentários explicativos:

```
# Inicializar uma máquina virtual (criar e provisionar) baseada no Vagrantfile no diretório atual
vagrant up


# Suspender (pausar) uma máquina virtual
vagrant suspend


# Desligar (parar) uma máquina virtual
vagrant halt


# Retomar uma máquina virtual suspensa
vagrant resume


# Destruir uma máquina virtual (remover completamente)
vagrant destroy


# Verificar o status das máquinas virtuais no diretório atual
vagrant status


# Acessar uma máquina virtual via SSH
vagrant ssh


# Listar todas as máquinas gerenciadas pelo Vagrant
vagrant global-status


# Recarregar uma máquina virtual (reiniciar a máquina mantendo o estado)
vagrant reload


# Recarregar uma máquina virtual de forma mais rápida (reiniciar sem recarregar o Vagrantfile)
vagrant reload --provision


# Prover (provisionar) uma máquina virtual novamente
vagrant provision


# Listar boxes disponíveis
vagrant box list


# Adicionar uma nova box ao Vagrant
vagrant box add <nome-da-box> <URL-ou-caminho-local>


# Remover uma box do Vagrant
vagrant box remove <nome-da-box>


# Exibir as configurações da máquina virtual no diretório atual
vagrant config


# Exibir informações sobre o Vagrant e a versão instalada
vagrant --version
```

Lembre-se de que esses comandos devem ser executados a partir do diretório onde está localizado o Vagrantfile correspondente à máquina virtual que você deseja gerenciar. Adicionalmente, o Vagrant oferece uma variedade de opções e argumentos para cada um desses comandos. Para obter mais detalhes sobre cada comando e suas opções, você pode usar o comando de ajuda vagrant --help ou consultar a documentação oficial do Vagrant.