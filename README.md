# ☁️ Laboratório AWS EC2

Este projeto foi desenvolvido com o objetivo de praticar conceitos relacionados à criação, configuração e gerenciamento de instâncias Amazon EC2.

Durante o laboratório foi criada uma instância Linux na AWS, realizado acesso remoto por SSH, instalado o servidor web Nginx e publicada uma página acessível pela Internet.

---

## 🎯 Objetivos

- Criar uma instância EC2;
- Configurar acesso SSH;
- Trabalhar com Security Groups;
- Instalar pacotes em uma instância Linux;
- Configurar um servidor web Nginx;
- Publicar uma página HTTP;
- Parar e iniciar uma instância;
- Observar alterações no endereço IP público;
- Validar a persistência dos dados;
- Documentar o processo utilizando GitHub.

---

## 🖥️ Ambiente utilizado

- Amazon EC2
- Amazon Linux 2023
- Instância `t3.micro`
- SSH
- Nginx
- PowerShell
- GitHub

---

## 🚀 Criação e inicialização da instância

Foi criada uma instância EC2 utilizando Amazon Linux 2023.

Após a inicialização, a instância ficou no estado de execução e as verificações de status foram concluídas com sucesso.

![Status da instância](images/03-status-instancia-saudavel.png)

---

## 🔐 Acesso à instância via SSH

Para realizar o acesso remoto à instância foi utilizado um par de chaves SSH.

O arquivo de chave privada `.pem` foi mantido apenas localmente e não foi publicado neste repositório.

A conexão foi realizada através do PowerShell utilizando um comando semelhante a:

```bash
ssh -i "lab-ec2-dio-key.pem" ec2-user@IP_PUBLICO

Na primeira conexão foi necessário confirmar a autenticidade do host.

Após a conexão, foram executados alguns comandos para validar o ambiente:
whoami
hostname
uname -a
df -h

Esses comandos permitiram verificar o usuário conectado, o hostname da máquina, informações do sistema operacional e o espaço em disco disponível.

Atualização do sistema
Antes da instalação dos serviços, o sistema foi atualizado com:
sudo dnf update -y

O sistema informou que não havia atualizações pendentes naquele momento.

Instalação do Nginx
O servidor web Nginx foi instalado utilizando:
sudo dnf install nginx -y

Depois, o serviço foi iniciado:
sudo systemctl start nginx

Também foi configurado para iniciar automaticamente junto com o sistema:
sudo systemctl enable nginx

O status do serviço foi verificado com:
sudo systemctl status nginx

O resultado confirmou:
Active: active (running)

 
🔐 Configuração do Security Group
Para permitir o acesso à instância foram utilizadas regras de entrada no Security Group.
As principais regras utilizadas no laboratório foram:
Tipo	Protocolo	Porta	Finalidade
SSH	TCP	22	Acesso remoto à instância
HTTP	TCP	80	Acesso ao servidor web


A porta 22 foi utilizada para a conexão SSH e a porta 80 para permitir o acesso ao Nginx através do navegador.
 
🌍 Teste da página padrão do Nginx
Após liberar a porta 80 no Security Group, o endereço IPv4 público da instância foi acessado pelo navegador.
A página padrão do Nginx foi exibida corretamente, confirmando que o servidor web estava funcionando e acessível pela Internet.
 
📝 Criação de uma página personalizada
A página padrão do Nginx foi substituída por uma página HTML simples criada para o laboratório.
O arquivo utilizado foi:
/usr/share/nginx/html/index.html

O conteúdo criado foi semelhante a:
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Laboratório AWS EC2</title>
</head>
<body>
    <h1>Laboratório AWS EC2</h1>
    <p>Instância EC2 criada e configurada com sucesso.</p>
    <p>Servidor web Nginx em execução no Amazon Linux 2023.</p>
</body>
</html>

Após atualizar a página no navegador, o conteúdo personalizado passou a ser exibido.
 
⏹️ Interrupção da instância
Depois dos testes iniciais, a instância foi interrompida através do console da AWS.
Esse procedimento permitiu observar o comportamento do ciclo de vida de uma instância EC2.
 
Durante o processo, a instância passou para o estado de interrupção.
 
Após alguns instantes, a instância ficou completamente interrompida.
 
▶️ Inicialização da instância
Depois da interrupção, a mesma instância foi iniciada novamente.
Foi observado que o endereço IPv4 público havia mudado.
Antes da interrupção:
34.228.70.37

Depois da nova inicialização:
34.203.199.62

Essa etapa demonstrou que o IPv4 público atribuído automaticamente pode mudar quando uma instância é interrompida e posteriormente iniciada novamente.
Mesmo com a mudança do IP público, os arquivos e configurações da instância permaneceram disponíveis.
A página personalizada continuou funcionando normalmente.
 
🔁 Validação do Nginx após reinício
Após a nova inicialização da instância, foi realizada outra conexão SSH utilizando o novo endereço IPv4 público.
O status do Nginx foi verificado novamente:
sudo systemctl status nginx

O serviço permaneceu em execução:
Active: active (running)

 
Também foi verificado se o serviço estava configurado para iniciar automaticamente:
systemctl is-enabled nginx

Resultado:
enabled

Por fim, foi realizado um teste diretamente de dentro da instância:
curl http://localhost

O comando retornou o HTML da página criada anteriormente, confirmando que o Nginx estava respondendo corretamente.
 
💡 Principais aprendizados
Durante o laboratório foi possível compreender na prática:
- Como criar uma máquina virtual utilizando Amazon EC2;
- Como realizar acesso remoto utilizando SSH;
- Como utilizar uma chave privada .pem;
- Como funcionam os Security Groups;
- Como liberar portas específicas para serviços;
- Como instalar pacotes em uma instância Linux;
- Como iniciar e gerenciar serviços utilizando systemctl;
- Como instalar e configurar o Nginx;
- Como disponibilizar uma página web pela Internet;
- Como interromper e iniciar uma instância EC2;
- Como o IP público pode mudar após uma interrupção;
- Como os arquivos da instância podem permanecer disponíveis após o processo de parada e inicialização;
- Como configurar um serviço para iniciar automaticamente com o sistema.
🔐 Boas práticas de segurança
Durante o laboratório foram adotados alguns cuidados importantes:
- O arquivo .pem não foi publicado no GitHub;
- Credenciais AWS não devem ser armazenadas em repositórios públicos;
- A porta SSH deve ser restrita sempre que possível;
- Apenas portas realmente necessárias devem ser liberadas;
- Informações sensíveis devem ser removidas de capturas de tela antes da publicação;
- Recursos AWS que não estiverem sendo utilizados devem ser interrompidos ou removidos para evitar cobranças desnecessárias.
📁 Estrutura do repositório
A estrutura utilizada neste projeto é:
aws-ec2-lab/
│
├── README.md
│
└── images/
    ├── 01-conexao-instancia-aws-redigida.png
    ├── 02-erro-conexao-navegador.png
    ├── 03-status-instancia-saudavel.png
    ├── 04-primeira-conexao-ssh.png
    ├── 05-acesso-ssh-comandos-linux.png
    ├── 06-atualizacao-amazon-linux.png
    ├── 07-nginx-active-running.png
    ├── 08-nginx-pagina-padrao.png
    ├── 09-pagina-personalizada-ec2.png
    ├── 10-parar-instancia-confirmacao.png
    ├── 11-instancia-interrompendo.png
    ├── 12-instancia-interrompida.png
    ├── 13-pagina-apos-reinicio-novo-ip.png
    ├── 14-security-group-regras-redigida.png
    ├── 15-nginx-apos-reinicio.png
    └── 16-validacao-curl-nginx.png

📚 Tecnologias e serviços utilizados
- AWS EC2
- Amazon Linux 2023
- Security Groups
- SSH
- PowerShell
- Nginx
- Linux
- GitHub
- Markdown
✅ Conclusão
Este laboratório permitiu aplicar na prática conceitos fundamentais relacionados ao gerenciamento de instâncias EC2 na AWS.
Além da criação da máquina virtual, foram realizadas atividades de acesso remoto, configuração de rede, instalação de serviços, publicação de uma página web e gerenciamento do ciclo de vida da instância.
Também foi possível observar comportamentos importantes, como a alteração do endereço IPv4 público após uma interrupção e a persistência dos dados armazenados na máquina.
A realização do projeto ajudou a consolidar conhecimentos básicos sobre computação em nuvem, Linux, redes, segurança e documentação técnica utilizando GitHub.
