# Introdução Cloud e AWS

Data Center e Recursos computacionais em uma empresa de Cloud provider, com serviços pagos conforme a demanda.

AWS, GCP, Azure, Oracle, Digital Ocean, IBM...

## Tipos de serviços

- On - Premisses (Local)
- Em núvem:
1. Infrastructure as a Service (IaaS)
2. Platform as a Service (PaaS)
3. Software as a Service (SaaS)

![Serviços e Responsabilidades](./services.png)

## AWS > Região > Zona de Disponibilidade (AZ)

Cada AZ é um Data Center, as AZs estão interligadas. Para ter resiliência e alta disponibilidade.

Por isso é importante utilizar multiplas AZs. Como contingência. Ou até múltiplas regiões (mas é muito caro).

Pensar na proximidade da região com os clientes (levantar requisitos).

## Criar conta
- Gratuita com cadastro de cartão internacional;
- Criar um grupo de administrador (AdministratorAccess);
- Criar um usuário com o IAM (Identity Access Manager);
- Com acesso ao console;
- Iam user;
- gerenciar a senha;
- Vincular ao grupo;

## Conceitos IAM
- Usuário: Qualquer user, que pode ter policies de acesso vinculadas, pertencer a user groups (com policies definidas), ou roles que vincula policies de controle de acesso a um serviço para outro serviço. 

## Configurações
- Região padrão: Utilizar o norte da Virginia para estudo (mais barata);
- More settings; Linguagem, região padrão. Dark mode...

## Serviços

### Criar uma rede virtual na aws - Rede isolada
- Serviço computacional geralmente fica em uma rede.
- VPC. Não utilizar a rede padrão, usar uma rede especialmente provisionada para a produção. Com o terraform podemos ter um padrão definido.
- Toda VPC é vinculada a uma região. É uma rede na nuvem.
1. Create VPC
- VPC only
- nome
- IPv4 manual: 10.0.0.0/16  # O "/16" diz que os 2 números da frente representam as redes (cada número tem 8bits (0->255), portanto 16 são 2), portanto, os de trás representam um dispositivo
- No IPv6
- Tenacy: Default
- Tags...
2. Dentro da VPC há uma subnet, onde ficará a EC2
- Create subnet
- A subnet é vinculada a uma VPC, logo, a uma região. Mas ela é ligada a uma AZ.
- Vincular a VPC criada
- Vincular a uma AZ (Dependendo da subnet a aplicação terá uma disponibilidade diferente): US East (N, Virginia) / us-east-1a
- VPC CiDR mantém
- subnet CIDR block: 10.0.1.0/24, derivando da rede (/24 usa os 3 numeros para a subrede)  # permite 256 dispositivos, mas a AWS reserva alguns IPs
3. Criar máquina virtual EC2
- Instances > Launch Instances
- name: ec2-cloud-intro
- escolher a imagem do SO: UBUNTU server
- Instance type (perfil da máquina): free tier t2.micro
- chave ssh
- Configurar a rede VPC e subnet: Clicar em Edit
- Habilitar Auto-assign public IP (pode gerar cobrança)
- Criar novo Firewall (security group)
- Regras: ALL anywhere (acesso total) para testar. MAS vou tentar deixar em SSH, protocolo TCP, porta 22, anywhere.
- Storage
4. Acessar a máquina
- Conceitos: subnet pública (com internet gateway) e subnet privada
    - Apenas saída: NAT gateway, assim só a máquina acessa a net
- Rede > VPC > internet gateway > create (poe o nome e tags)
- Actions > Attach to a VPC
- Route Table: Faz o direcionamento da rede para o internet gateway
    - Criar e vincular a VPC
    - Edit Routes: 
    - Adicionar uma rota (endereços): 0.0.0.0/0 Internet Gateway igw-nnome na lupa
    - Subnet associations: Associar a subnet
- Terminal:
`~/.ssh$ chmod 400 f41-rggn-admin.pem` # Permissões da chave
`~/.ssh$ ssh -i f41-rggn-admin.pem ubuntu@34.229.192.236` # Conectar na máquina e o terminal já fica no ubuntu
- Pode dar update... instalar docker e subir a aplicação
- Pode se conectar pelo IP público no browser.

