## ![image](https://github.com/user-attachments/assets/fd2795df-e81f-4f79-9d76-a193c8814f0b)
# Arquitetura Híbrida

Este repositório contém a solução de arquitetura híbrida (on-premises e cloud) para o sistema de fluxo de caixa da empresa XPTO, desenvolvida como parte do desafio de arquitetura de soluções de infraestrutura.

## Visão Geral

A solução proposta implementa uma arquitetura híbrida para os serviços de fluxo de caixa da XPTO:
- **Serviço de controle de lançamentos**: Mantido on-premises com replicação para a nuvem
- **Serviço de consolidação diária**: Migrado para AWS com alta disponibilidade e escalabilidade

A arquitetura foi projetada para garantir que o serviço de controle de lançamento não fique indisponível se o sistema de consolidado diário cair, e para suportar picos de até 50 requisições por segundo no serviço de consolidado diário com no máximo 5% de perda de requisições.

## Estrutura do Repositório

```
fluxo-de-caixa/
├── docs/                      # Documentação detalhada
│   ├── arquitetura.md         # Descrição da arquitetura
│   ├── dimensionamento.md     # Dimensionamento de recursos
│   ├── finops.md              # Estratégias de FinOps
│   ├── disaster-recovery.md   # Plano de DR
│   ├── monitoramento.md       # Monitoramento e observabilidade
│   ├── seguranca.md           # Segurança da solução
│   └── modelo-osi.md          # Aplicação do modelo OSI
├── diagramas/                 # Diagramas de topologia
│   ├── arquitetura_geral.py   # Código Python para gerar diagrama
│   └── arquitetura_geral.png  # Diagrama de arquitetura
├── infra/                     # Código de infraestrutura
│   ├── modules/               # Módulos Terraform
│   │   ├── vpc/               # Módulo de rede
│   │   ├── security/          # Módulo de segurança
│   │   ├── compute/           # Módulo de computação
│   │   ├── database/          # Módulo de banco de dados
│   │   └── monitoring/        # Módulo de monitoramento
│   ├── ansible/               # Configuração do ambiente on-premises
│   ├── main.tf                # Arquivo principal do Terraform
│   ├── variables.tf           # Variáveis do Terraform
│   └── outputs.tf             # Outputs do Terraform
└── README.md                  # Este arquivo
```

## Componentes Principais

### Ambiente On-Premises
- Serviço de controle de lançamentos
- Banco de dados PostgreSQL primário
- Conexão VPN Site-to-Site com AWS

### Ambiente AWS
- VPC com subnets públicas e privadas em múltiplas zonas de disponibilidade
- Serviço de consolidação diária implementado com Lambda e EC2 Spot
- RDS PostgreSQL como réplica do banco on-premises
- DynamoDB para cache de dados
- Application Load Balancer para distribuição de tráfego
- Auto Scaling para ajuste automático de capacidade
- CloudWatch para monitoramento e alertas

## Requisitos Atendidos

### Requisitos Obrigatórios
- ✅ Dimensionamento de recursos (CPU, memória, estratégias de escalabilidade)
- ✅ Estratégias de FinOps para controle de custos
- ✅ Diagrama de topologia completo
- ✅ Justificativas técnicas para cada escolha
- ✅ Automação com IaC (Terraform e Ansible)

### Requisitos Diferenciais
- ✅ Plano de DR (Disaster Recovery)
- ✅ Monitoramento e observabilidade
- ✅ Aplicação do modelo OSI

## Como Usar

### Pré-requisitos
- AWS CLI configurado
- Terraform v1.0+
- Ansible v2.9+
- Python 3.8+ (para diagramas)

### Implantação

1. Clone o repositório:
```bash
git clone https://github.com/darionewton7/fluxo-de-caixa.git
cd fluxo-de-caixa
```

2. Inicialize o Terraform:
```bash
cd infra
terraform init
```

3. Personalize as variáveis:
```bash
cp terraform.tfvars.example terraform.tfvars
# Edite terraform.tfvars com seus valores
```

4. Aplique a infraestrutura:
```bash
terraform apply
```

5. Configure o ambiente on-premises:
```bash
cd ansible
ansible-playbook -i inventory.ini playbook.yml
```

## Documentação Detalhada

Para informações mais detalhadas sobre cada componente da solução, consulte os documentos na pasta `docs/`:

- [Arquitetura](docs/arquitetura.md)
- [Dimensionamento](docs/dimensionamento.md)
- [FinOps](docs/finops.md)
- [Disaster Recovery](docs/disaster-recovery.md)
- [Monitoramento](docs/monitoramento.md)
- [Segurança](docs/seguranca.md)
- [Modelo OSI](docs/modelo-osi.md)

## Licença

Este projeto está licenciado sob a licença MIT - veja o arquivo LICENSE para detalhes.
