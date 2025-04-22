<h1 align="center">

[![image](https://github.com/user-attachments/assets/4dbb8107-d52f-4cb9-94f3-68310a4d98b2)](https://loboguara.app/docs/index.html)

Guia de Instalação do Lobo Guará

</h1>

<h4 align="center">

Instruções para a instalação do Lobo Guará e o primeiro acesso à plataforma.

</h4>

## Pré-Requisitos

Antes de iniciar a instalação, verifique se o seu servidor atende aos seguintes requisitos:

- **Sistema operacional:** Ubuntu 24.04 e Red Hat 9.4
- **Mínimo de disco:** 32 GB
- **Mínimo de memória RAM:** 8 GB
- **Mínimo de CPU:** 4 CPU

<br>

## Instalação do Python 3.12

**1. Atualizar o servidor**

Antes de iniciar a instalação, certifique-se de que o servidor está atualizado. Isso garante que os pacotes necessários sejam instalados corretamente.
```bash
sudo apt update && sudo apt upgrade -y
```

**2. Instalar o pré-requisito para adicionar PPAS personalizados**

Este pacote permite adicionar novos repositórios ao APT.
```bash
sudo apt install software-properties-common -y
```

**3. Adicionar o repositório deadsnakes**

Esse repositório contém versões mais recentes do Python.
```bash
add-apt-repository ppa:deadsnakes/ppa
```

![image](https://github.com/user-attachments/assets/56860a38-dd0c-4768-a224-1fc7e3b57c4d)

Pressione a tecla **ENTER** para continuar a instalação.

**4. Atualizar a lista de pacotes novamente**

Atualize o cache de pacotes do sistema após adicionar o novo repositório.
```bash
sudo apt update && sudo apt upgrade -y
```

**5. Instalar o Python 3.12**

Instala a versão necessária do Python para o projeto.
```bash
sudo apt install python3.12 -y
```

**6. Instalar o PIP, pois não vem instalado por padrão**

O gerenciador de pacotes Python não vem por padrão nessa versão, então é necessário instalá-lo manualmente.
```bash
curl -sS https://bootstrap.pypa.io/get-pip.py | python3.12
```

**7. Instalar as dependências do Python**

Esses pacotes são importantes para ambientes virtuais e desenvolvimento.
```bash
sudo apt-get install python3.12 python3.12-venv python3.12-dev -y
sudo apt update
```

**8. Verificar a versão do Python**

Confirme se o Python 3.12 foi instalado com sucesso.
```bash
python3.12 --version
```

<br>

## Instalação e Configuração do PostgreSQL

**Nota:** É recomendado realizar as ações administrativas a partir do diretório raiz.
```bash
cd /
```

**1. Instalar o PostgreSQL**

Instala o banco de dados relacional necessário para o Lobo Guará.
```bash
sudo apt install postgresql postgresql-contrib -y
```

**2. Criar banco de dados e usuário**

Cria o banco guaradb e o usuário guarauser com uma senha segura.

**Nota:** Em **your_password**, coloque uma senha segura para acessar o banco de dados guaradb.

```bash
sudo -u postgres psql -c "CREATE DATABASE guaradb;"
sudo -u postgres psql -c "CREATE USER guarauser WITH PASSWORD 'your_password';"
```

**3. Conceder permissões ao usuário**

Dá ao usuário todas as permissões necessárias no banco.
```bash
sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE guaradb TO guarauser;" && \
sudo -u postgres psql -d guaradb -c "ALTER SCHEMA public OWNER TO guarauser;" && \
sudo -u postgres psql -d guaradb -c "GRANT USAGE ON SCHEMA public TO guarauser;" && \
sudo -u postgres psql -d guaradb -c "GRANT CREATE ON SCHEMA public TO guarauser;" && \
sudo -u postgres psql -d guaradb -c "GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO guarauser;" && \
sudo -u postgres psql -d guaradb -c "GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO guarauser;" && \
sudo -u postgres psql -d guaradb -c "GRANT ALL PRIVILEGES ON ALL FUNCTIONS IN SCHEMA public TO guarauser;" && \
sudo -u postgres psql -d guaradb -c "ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON TABLES TO guarauser;" && \
sudo -u postgres psql -d guaradb -c "ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON SEQUENCES TO guarauser;" && \
sudo -u postgres psql -d guaradb -c "ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON FUNCTIONS TO guarauser;"
```

**4. Criar extensão de indexação**

Essa extensão melhora a performance de buscas por similaridade textual.
```bash
sudo -u postgres psql -d guaradb -c "CREATE EXTENSION IF NOT EXISTS pg_trgm;"
```

<br>

## Instalar e Configurar Redis

**1. Instalar o Redis**

O Redis é necessário para operações assíncronas e cache.
```bash
sudo apt install redis-server -y
sudo systemctl start redis-server
sudo systemctl enable redis-server
```

<br>

## Instalar Git

**1. Instalar Git**

O Git é usado para clonar o repositório da aplicação.
```bash
sudo apt install git -y
```

<br>

## Instalar ferramentas de desenvolvimento

**1. Instalar pacotes essenciais**

Necessários para compilar pacotes nativos e lidar com arquivos comprimidos.
```bash
sudo apt install build-essential zip -y
```

<br>

## Instalar Google Chrome e ChromeDriver

**1. Instalar o Chrome e o driver compatível**

Esses componentes são usados por funcionalidades de automação da plataforma.
```bash
sudo mkdir -p /opt/loboguara/bin/
wget -O /tmp/google-chrome.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i /tmp/google-chrome.deb || sudo apt-get install -f -y
sudo ln -sf /usr/bin/google-chrome /opt/loboguara/bin/google-chrome
wget -O /tmp/chromedriver.zip https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/131.0.6778.69/linux64/chromedriver-linux64.zip
sudo unzip /tmp/chromedriver.zip -d /tmp/
sudo mv /tmp/chromedriver-linux64/ /opt/loboguara/bin/chromedriver_dir
sudo chmod +x /opt/loboguara/bin/chromedriver_dir/chromedriver
sudo ln -sf /opt/loboguara/bin/chromedriver_dir/chromedriver /opt/loboguara/bin/chromedriver
```

<br>

## Instalar Subfinder

**1. Instalar o Subfinder**

Ferramenta utilizada para encontrar subdomínios de forma automatizada.
```bash
sudo wget -O /tmp/subfinder.zip https://github.com/projectdiscovery/subfinder/releases/download/v2.6.6/subfinder_2.6.6_linux_amd64.zip
sudo unzip /tmp/subfinder.zip -d /tmp/
sudo mv /tmp/subfinder /opt/loboguara/bin/
sudo chmod +x /opt/loboguara/bin/subfinder
```

<br>

## Instalar FFUF

**1. Instalar o FFUF**

Usado para fuzzing de diretórios e rotas em aplicações web.
```bash
sudo wget -O /tmp/ffuf.tar.gz https://github.com/ffuf/ffuf/releases/download/v2.0.0/ffuf_2.0.0_linux_amd64.tar.gz
sudo tar -xvzf /tmp/ffuf.tar.gz -C /tmp/
sudo mv /tmp/ffuf /opt/loboguara/bin/
sudo chmod +x /opt/loboguara/bin/ffuf
```

<br>

## Clonar o Repositório Lobo Guará

**1. Clonar o repositório**

Baixa o código-fonte do projeto para seu servidor.
```bash
git clone https://github.com/olivsec/loboguara.git
cd loboguara/
```

<br>

## Configurar o arquivo config.py

**1. Editar o arquivo de configuração**

Abra o arquivo config.py e ajuste as variáveis conforme seu ambiente.
```bash
nano server/app/config.py
```

![image](https://github.com/user-attachments/assets/effe36c2-70fe-495b-bb53-2fc906439546)


Dentro do arquivo, você precisará configurar as seguintes variáveis:

- **SECRET_KEY** 
- **SQLALCHEMY_DATABASE_URI**
- **API_ACCESS_TOKEN**
<br>


**SECRET_KEY**

Usada para criptografar sessões e outros dados sensíveis. É fundamental que você gere uma chave secreta forte e única. 
<br>
<br>

**SQLALCHEMY_DATABASE_URI**

Define a URI de conexão com o banco de dados. Por padrão, ela está configurada da seguinte maneira:
```bash
'postgresql://guarauser:YOUR_PASSWORD_HERE@localhost/guaradb?sslmode=disable'
```
Substitua **YOUR_PASSWORD_HERE** pela senha do seu banco de dados PostgreSQL.
<br>
<br>

**API_ACCESS_TOKEN**

Para configurar o API_ACCESS_TOKEN, você precisará criar uma conta na plataforma Lobo Guará. Acesse o site oficial e faça o login:

https://loboguara.olivsec.com.br/login?next=%2F

Após o login, você poderá obter o seu token de acesso e adicioná-lo na variável **API_ACCESS_TOKEN** dentro do arquivo config.py.
<br>
<br>

**Exemplo de configuração**

Após ajustar as variáveis conforme seu ambiente, o arquivo config.py ficará da seguinte maneira:

![image](https://github.com/user-attachments/assets/c77beec7-28e4-4690-9004-6ee21836a67e)


<br>

## Executar o instalador

**1. Executar o script de instalação**

Concede permissão de execução e inicia o processo automatizado.
```bash
sudo chmod +x ./install.sh
sudo ./install.sh
```

Após a execução do instalador, irá aparecer a seguinte imagem e mensagem avisando que a instalação foi feita com sucesso.

![image](https://github.com/user-attachments/assets/52fe13a7-fb75-45c0-82d3-8beb0988c222)

![image](https://github.com/user-attachments/assets/2b96bd75-cedd-499e-8cdf-a8e43f2e0f1a)


**2. Iniciar a aplicação**

Após a instalação, inicie o serviço manualmente.
```bash
sudo -u loboguara /opt/loboguara/start.sh
```

![image](https://github.com/user-attachments/assets/cd5b9bc0-f43f-4b17-88ea-892b843c8fa9)


<br>

## Primeiro acesso à plataforma

**1. Acessar a interface web**

Abra o navegador e acesse a interface web do Lobo Guará utilizando o IP da sua máquina seguido da porta 7405.
```bash
http://<IP-do-Servidor:7405>
```


