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


## Instalação do Python 3.12

**1. Atualizar o servidor**

Antes de iniciar a instalação, certifique-se de que o servidor está atualizado. Isso garante que os pacotes necessários sejam instalados corretamente.
```bash
sudo apt update && sudo apt upgrade -y
```

**2. Instalar o pré-requisito para adicionar PPAS personalizados**

```bash
sudo apt install software-properties-common -y
```

**3. Adicionar o deadsnakes PPA à lista de fontes de gerenciamento de pacotes APT**

```bash
add-apt-repository ppa:deadsnakes/ppa
```

![image](https://github.com/user-attachments/assets/56860a38-dd0c-4768-a224-1fc7e3b57c4d)


Pressione a tecla **ENTER** para continuar a instalação.


**4. Atualizar a lista de pacotes disponíveis**

```bash
sudo apt update && sudo apt upgrade -y
```

**5. Instalar o Python 3.12**

```bash
sudo apt install python3.12 -y
```

**6. Instalar o PIP, pois não vem instalado por padrão**

```bash
curl -sS https://bootstrap.pypa.io/get-pip.py | python3.12
```

**7. Instalar as dependências do Python**

```bash
sudo apt-get install python3.12 python3.12-venv python3.12-dev -y
sudo apt update
```

**8. Verificar se o Python 3.12 foi instalado corretamente**

```bash
python3.12 --version
```


## Instalação e Configuração do PostgreSQL

**Nota:** para fazer a instalação e a configuração do PostgreSQL, faça no diretório raiz.
```bash
cd /
```

**1. Instalar o PostgreSQL**

```bash
sudo apt install postgresql postgresql-contrib -y
```

**2. Criar o banco de dados e o usuário no PostgreSQL**

**Nota:** O nome do banco de dados vai se chamar **guaradb** e usuário do banco de dados vai se chamar **guarauser**.

Em **your_password**, coloque uma senha segura para acessar o banco de dados guaradb.

```bash
sudo -u postgres psql -c "CREATE DATABASE guaradb;"
sudo -u postgres psql -c "CREATE USER guarauser WITH PASSWORD 'your_password';"
```

**3. Conceder as permissões necessárias para o usuário guarauser**

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

```bash
sudo -u postgres psql -d guaradb -c "CREATE EXTENSION IF NOT EXISTS pg_trgm;"
```


## Instalar e Configurar Redis

**1. O Redis é necessário para algumas funcionalidades do sistema**

```bash
sudo apt install redis-server -y
sudo systemctl start redis-server
sudo systemctl enable redis-server
```


## Instalar Git

**1. O Git é necessário para clonar o repositório Lobo Guará**

```bash
sudo apt install git -y
```

## Instalar ferramentas de desenvolvimento

**1. Instalar o build-essential e o zip, que são usados para compilar e descompactar pacotes**

```bash
sudo apt install build-essential zip -y
```

## Instalar Google Chrome e ChromeDriver (versão 131.0.6778.69)

**1. Google Chrome e ChromeDriver são necessários para funcionalidades de automação dentro do Lobo Guará**

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

## Instalar Subfinder (versão 2.6.6)

**1. O Subfinder é uma ferramenta de descoberta de subdomínios**

```bash
sudo wget -O /tmp/subfinder.zip https://github.com/projectdiscovery/subfinder/releases/download/v2.6.6/subfinder_2.6.6_linux_amd64.zip
sudo unzip /tmp/subfinder.zip -d /tmp/
sudo mv /tmp/subfinder /opt/loboguara/bin/
sudo chmod +x /opt/loboguara/bin/subfinder
```

## Instalar FFUF

**1. O FFUF é utilizado para fuzzing de URL e diretórios**

```bash
sudo wget -O /tmp/ffuf.tar.gz https://github.com/ffuf/ffuf/releases/download/v2.0.0/ffuf_2.0.0_linux_amd64.tar.gz
sudo tar -xvzf /tmp/ffuf.tar.gz -C /tmp/
sudo mv /tmp/ffuf /opt/loboguara/bin/
sudo chmod +x /opt/loboguara/bin/ffuf
```

## Clonar o Repositório Lobo Guará

**1. Agora, baixe o projeto Lobo Guará diretamente do GitHub**

```bash
git clone https://github.com/olivsec/loboguara.git
cd loboguara/
```

## Configurar o arquivo config.py

**1. No diretório loboguara você encontrará o arquivo de configuração em server/app/config.py**

```bash
nano server/app/config.py
```

Nessa parte, será configurada as seguintes variáveis:

- SECRET_KEY
- SQLALCHEMY_DATABASE_URI
- API_ACCESS_TOKEN

Em SECRET_KEY, gere uma chave secreta segura, nesse guia será usado a chave yS1[t4l6k+

Na SQLALCHEMY_DATABASE_URI, será configurado o acesso ao banco de dados, no seguinte script que vem por padrão:
'postgresql://guarauser:YOUR_PASSWORD_HERE@localhost/guaradb?ssl mode=disable'.

Substitua YOUR_PASSWORD_HERE pela senha criada para seu banco de dados.

Já a API_ACCESS_TOKEN é possível obtê-la criando uma conta na própria plataforma do Lobo Guará no seguinte site:

https://loboguara.olivsec.com.br/login?next=%2F

Após fazer os ajustes nas variáveis ficará da seguinte forma:


## Executar o instalador

**1. Depois de configurar o arquivo config.py, execute o script install.sh para concluir a instalação**

```bash
sudo chmod +x ./install.sh
sudo ./install.sh
```

Após a execução do instalador, irá aparecer a seguinte imagem e mensagem avisando que a instalação foi feita com sucesso


**2. Iniciar a aplicação**

```bash
sudo -u loboguara /opt/loboguara/start.sh
```




















