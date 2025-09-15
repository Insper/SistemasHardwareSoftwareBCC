# Linux: Instalação e configuração 

## Download e particionar

Para fazer o download do Linux Ubuntu 22.04.5 LTS acesse o link [https://releases.ubuntu.com/jammy/](https://releases.ubuntu.com/jammy/), e [nesse VÍDEO](https://www.youtube.com/watch?v=VK4eCi7ktCE) explica como fazer dual boot de uma máquina com Windows e Linux.

## VM usando Virtualbox

### Shared Folder

Uma boa maneira de transferir arquivos entre a VM e a máquina hospedeira (ambas as direções) é criar uma pasta compartilhada no Virtualbox.

Acesse [este LINK](https://carleton.ca/scs/tech-support/troubleshooting-guides/creating-a-shared-folder-in-virtualbox/) para mais informações!

### Shared Clipboard

Também é possível fazer com que algo copiado (CTRL + C) no sistema principal possa ser colado no Ubuntu (CTRL + V na máquina Virtual), e vice-versa!

Procure no Google `virtualbox enable shared clipboard`.

## WSL

O Subsistema do Windows para Linux (WSL) é um recurso do Windows que permite executar um ambiente Linux em seu computador Windows, sem a necessidade de uma máquina virtual separada ou inicialização dupla ([Microsoft Learn](https://learn.microsoft.com/pt-br/windows/wsl/about)).
O WSL 2 é o tipo de distribuição padrão ao instalar uma distribuição do Linux. O WSL 2 usa a tecnologia de virtualização para executar um kernel do Linux dentro de uma VM (máquina virtual) de utilitário leve. As distribuições do Linux são executadas como contêineres isolados dentro da VM gerenciada do WSL 2.
[Como instalá-lo](https://learn.microsoft.com/pt-br/windows/wsl/install).

### Passo a passo para configurar WSL para aula de Sishard

1. Entrar no explorador de arquivos e clicar no ícone do Linux (caso não tenha este ícone, rodar ```wsl —install``` no terminal do Windows) para garantir que apenas o docker-desktop está instalado como distribuição no WSL. Caso mais alguma esteja instalada, usar o comando: ```wsl --unregister <Distribution Name>```, para desinstalar a distribuição. 

2. No Terminal do Windows escrever: ```wsl -l -o```, para ver as distribuições disponíveis para instalação.

3. Usar o comando: ```wsl --install -d Ubuntu-22.04```, no Terminal do Windows para instalar a distribuição Ubuntu na versão necessária para as aulas de sishard.

4. Usar o comando: ```lsb_release -a```, no Terminal do Ubuntu (será tudo no Terminal do Ubuntu daqui para frente) para verificar se a versão é a 22.04 mesmo.

5. Usar os comando: ```sudo apt update``` e ```sudo apt upgrade``` para atualizar os pacotes do Ubuntu.

6. Usar o comando: ```sudo apt-get gdb``` para instalar o ***gdb**.

7. Usar o comando: ```git --version``` para verificar se o git está instalado. Caso não esteja, instalar usando: ```sudo apt-get install git```

8. Configurar o **git** com os comandos: ```git config --global user.name "Seu nome"``` e ```git config --global user.email "seu-email"```

9. Entrar no VSCode e instalar a extensão do WSL; ele irá aparecer nos recomandedados.

10. Entrar no Terminal do Ubuntu e escrever: ```code``` para abrir o VSCode com o WSL, método mais eficiente já que temos mais de uma distribuição no wsl.

11. Instalar o **gcc** e o **make** no Ubuntu com os comandos: ```sudo apt install gcc make```

12. Clonar o repositório de sishard e fazer um commit aleatório para confirmar que está tudo configurado certinho. Testar também o gdb no **hackerlab** e o make na **atv4** (quando estiverem disponíveis).

