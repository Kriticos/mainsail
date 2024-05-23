# Configuração do WSL e Instalação do Docker

## WSL (Windows Subsystem for Linux)

1. Abra o PowerShell como Administrador.
2. Execute os seguintes comandos (dependendo do computador e da internet, o processo pode demorar):
    ```bash
    wsl --install
    wsl --install ubuntu
    ```
3. Reinicie o computador.
4. Após reiniciar e fazer login no Windows, geralmente aparecerá uma tela (terminal) solicitando um nome de usuário:
    - Coloque o nome "mainsail" e crie uma senha quando solicitado.
5. Se ao fazer login no Windows a janela do terminal não aparecer, abra o terminal pelo menu iniciar (pode ser o cmd) e execute:
    ```bash
    wsl -d ubuntu
    ```

## Ubuntu (WSL)

1. Às vezes, o Ubuntu é carregado, mas não consegue navegar na internet. Para testar, execute:
    ```bash
    sudo apt update
    ```
2. Se o comando acima não for executado corretamente, execute:
    ```bash
    sudo rm /etc/resolv.conf
    sudo bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
    sudo bash -c 'echo "[network]" > /etc/wsl.conf'
    sudo bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf'
    sudo chattr +i /etc/resolv.conf
    ```

## Instalação do Docker

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
