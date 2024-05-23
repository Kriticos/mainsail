# Guia para Configurar o WSL e Instalar o Docker

## Passo 1: Configurar o WSL (Windows Subsystem for Linux)

1. Abra o "Prompt de Comando" como Administrador. Para fazer isso, clique com o botão direito no ícone do "Prompt de Comando" e escolha "Executar como administrador".
   
2. Digite o seguinte comando e pressione Enter: 
   ```powershell
   wsl --install
   ```
   3. Reinicie o computador.

## Passo 2: Instalar o Ubuntu
1. Abra o Powershell e digite o codigo abaixo e presione enter novamente
    ```Powershell
    wsl --install ubuntu 
    ```
2. Assim que terminar a instalação sera solicitado um nome de usuário. Digite `mainsail` como nome de usuário e crie uma senha quando solicitado.

3. Para sair do Ubuntu digite:
   ```bash
   exit
   ```

## Passo 3: Instalar o Docker dentro do Ubuntu

1. Abra o "Powershell" como Administrador novamente.

2. Copie e cole cada um dos comandos abaixo, pressionando Enter após cada um:
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
1. No terminal do Ubuntu(WSL), cole o seguinte comando e pressione Enter: 
   
   ```bash
   docker pull ghcr.io/mainsail-crew/mainsail.
    ```

## Passo 5: Criar o arquivo "config.conf" para o conteiner do Mainsail
2. No terminal do Ubuntu(WSL)
    ```bash
    nano /home/mainsail/config.json e pressione Enter.
    ```
Cole o seguinte conteúdo dentro do arquivo "config.json" e pressione "Ctrl + X" depois "Y" para salvar e "Enter" para sair:
```bash
   {
    "instancesDB": "browser"
   }
```

## Passo 6: Iniciar um Container do Mainsail
1. No terminal do Ubuntu(WSL), cole o seguinte comando e pressione "Enter":
    ```bash
    sudo docker run --name mainsail -v "/home/mainsail/config.json:/usr/share/nginx/html/config.json" -p
    ```

Pronto o Mainsail esta sendo executado.