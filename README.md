# Documentação do Script de Instalação e Configuração

## Visão Geral

Este script automatiza a instalação e configuração de ferramentas como **GNS3**, **Docker** e permissões para usuários LDAP,  para as aulas de Redes de Computadores, Administração de Redes de Computadores, Cibersegurança e Administração de Sistemas Operacionais e Ambientes Virtualizados do curso de BCC da UTFPR-CM.

---

## Estrutura do Script

### Funções Auxiliares

#### `gDriveDown()`
- Faz o download de um arquivo do Google Drive.
- **Parâmetros**:
  - `$1`: ID do arquivo no Google Drive.
  - `$2`: Nome do arquivo de saída.
- **Detalhes**:
  - Verifica se o arquivo já existe.
  - Cria o diretório de destino caso ele não exista.
  - Utiliza `wget` para baixar o arquivo, gerenciando cookies automaticamente.

#### `verifyFile()`
- Verifica se um arquivo existe.
- **Parâmetros**:
  - `$1`: Caminho do arquivo.
- **Comportamento**:
  - Caso o arquivo não seja encontrado, o script é encerrado.

---

### Execução Principal

#### 1. Verificação de Permissões
- Garante que o script está sendo executado como `root` ou com `sudo`.
- Caso contrário, o script é encerrado.

#### 2. Configuração de Permissões para Diretórios LDAP
- Ajusta permissões para os diretórios:
  - `/home/usuarios/`
  - `/home/usuarios/pessoas/`
- Copia arquivos de configuração para:
  - `/etc/pam.d/common-session`
  - `/etc/login.defs`

#### 3. Instalação e Configuração do Docker
- Instala pacotes essenciais:
  - `apt-transport-https`, `ca-certificates`, `curl`, `gnupg`, entre outros.
- Adiciona o repositório oficial do Docker.
- Instala o Docker e configura:
  - Arquivos copiados: `nsswitch.conf`, `group.conf`, `common-auth`, `daemon.json`.
  - Reinicia o serviço Docker para aplicar configurações.

#### 4. Instalação e Configuração do GNS3
- Instala pacotes:
  - `gns3-server`, `gns3-gui`, `dynamips`, `vpcs`, `ubridge`, entre outros.
- Configura diretórios e permissões:
  - Cria `/etc/gns3`.
  - Ajusta permissões e donos para `/var/gns3/`.
- Baixa imagens do Google Drive usando `gDriveDown()`:
  - Exemplo:
    - Arquivo: `c7200-adventerprisek9-mz.124-24.T5.bin`
    - ID: `1uR5e3nsfgvpRE9bNXSok4rZO4HCkqjET`
- Configura o executável do GNS3:
  - Renomeia `/usr/bin/gns3` para `/usr/bin/gns3-gui`.
  - Cria um novo executável em `/usr/bin/gns3`.
- Configura o serviço do GNS3 para inicializar no boot:
  - Copia o arquivo de serviço para `/etc/systemd/system/gns3.service`.
  - Reinicia e habilita o serviço.

---

## Exemplo de Uso

1. Certifique-se de executar o script como `root` ou com `sudo`.
2. Execute o script:
   ```bash
   ./script.sh
3. Siga as mensagens exibidas no terminal.

---
## Observações

1. Pré-requisitos:
 - Sistema baseado em Debian/Ubuntu.
  - Conexão com a internet para baixar pacotes e imagens.
2. Cuidado:
  - O script faz alterações permanentes no sistema, como permissões e configurações de arquivos.

---
## Estrutura do Repositório

- etc/: Contém os arquivos de configuração necessários.
- bin/: Contém scripts auxiliares, como o novo executável do GNS3.
- gns3/: Diretório com imagens, appliances e configurações adicionais.

---

Desenvolvido pelo professor https://github.com/luizsantos/
