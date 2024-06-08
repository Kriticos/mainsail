# Mainsail no Ubuntu(WSL) com Docker

O procedimento abaixo foi efetuado no Windows 11 23H2

## Passo 1: Configurar o WSL (Windows Subsystem for Linux)

1.1. Abra o "Prompt de Comando" como Administrador. Para fazer isso, clique com o botão direito no ícone do "Prompt de Comando" e escolha "Executar como administrador".

1.2. Digite o seguinte comando e pressione Enter: 

```powershell
    wsl --install
```
1.3. Reinicie o computador.

**OBS:** Geralmetne ele ja faz a instalação do Ubuntu. Apos reiniciar sera soliciatado um usuario e senha.

## Passo 2: Instalar o Ubuntu

**OBS:** Caso durante o passo anterior não tenha sido feito o download do Ubuntu faça o passo 2.1, caso contrario pule para o 2.2

2.1. Abra o Powershell e digite o codigo abaixo e presione enter novamente
```Powershell
    wsl --install ubuntu 
```
2.2. Assim que terminar a instalação sera solicitado um nome de usuário. Digite `mainsail` como nome de usuário e crie uma senha quando solicitado.

2.3. Para sair do Ubuntu digite:

```bash
    exit
```

## Passo 3: Instalar o Docker dentro do Ubuntu

Geralmente apos a instalação do Ubunto ele não acessa a internet pra resolver siga os pasos abaixo:

```bash
sudo rm /etc/resolv.conf
sudo bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
sudo bash -c 'echo "[network]" > /etc/wsl.conf'
sudo bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf'
sudo chattr +i /etc/resolv.conf
```

3.1. Abra o "Powershell" como Administrador novamente.

3.2. Copie e cole cada um dos comandos abaixo, pressionando Enter após cada um:
```bash
    sudo apt-get update
```

```bash
    sudo apt-get install ca-certificates curl
```

```bash
    sudo install -m 0755 -d /etc/apt/keyrings
```

```bash
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

```bash
    sudo chmod a+r /etc/apt/keyrings/docker.asc
```

```bash
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
 ```

```bash
    sudo apt-get update
 ```

```bash
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
 ```

## Passo 4: Baixar a Imagem do Mainsail
4.1. No terminal do Ubuntu(WSL), cole o seguinte comando e pressione Enter: 

```bash
   sudo docker pull ghcr.io/mainsail-crew/mainsail
```

## Passo 5: Criar o arquivo "config.conf" para o conteiner do Mainsail
4.2. No terminal do Ubuntu(WSL)

```bash
    nano /home/mainsail/config.json
```

Cole o seguinte conteúdo dentro do arquivo "config.json" e pressione "Ctrl + X" depois "Y" para salvar e "Enter" para sair:

```bash
{
    "instancesDB": "browser"
}
```

## Passo 5: Iniciar um Container do Mainsail
5.1. No terminal do Ubuntu(WSL), cole o seguinte comando e pressione "Enter":

```bash
    sudo docker run --name mainsail -v "/home/mainsail/config.json:/usr/share/nginx/html/config.json" -p
```

Pronto o Mainsail esta sendo executado, pode fechar o terminal.

## Bonus 01
Executar de forma automatica o Mainsail sempre que vc logar no Windows

B.1. Abra o bloco de notas e cole o texto abaixo
```
    wsl -d ubuntu -u root
```

B.2. Salve dentro da pasta **"C:\Scripts"** com o nome de mainsail.ps1

B.3. Abra o agendador de tarefas do windows

B3.1. Click com o botão direito do mouse em **"Biblioteca do Agendador de Tarefas e escolha"** e escolha **"Criar tarefa basica"**

B.3.2. De um nome depois click em "Avançar"

B.3.3. Escolha **"Ao fazer logon"** e click em "Avançar"

B.3.4. Escolha **"Iniciar um programa"** e click em "Avançar"

B.3.5. Em **"Programa/Script"** coloque
```
    C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
```

B.3.6. Em **"Adicione argumentos"** (opcional)
```
    -FILE "C:\Scripts\mainsail.ps1"
```

B.3.7. Abrindo o Ubuntu - Abra o Powershell e digite
```
    wsl -d ubuntu -u root
```

B.3.8. Configurando o arquivo wsl.conf
```
    nano /etc/wsl.conf
```

B.3.9. Adicione ao final do arquivo wsl.conf depois pressione "Ctrl + X" depois "Y" para salvar e "Enter" para sair:
```
    command = "service docker start"
```

B.3.10. Saia do terminal
```
    exit
```

## Bonus 02
Configurando quantidade de memoria e processador do WSL
 
Abra o Bloco de Notas e digite:

```notepad
    [wsl2]
    memory=4GB
    processors=4
```

OBS: Altere o valor de memoria e processador conforme necessario.

Salve o arquivo dentro da pasta do seu usuario do Windows "C:\Usuarios\NOME_DO_SEU_USUARIO\" com o nome ".wslconfig"