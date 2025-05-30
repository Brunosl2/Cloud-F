# Roteiro 3 - Deploy de Aplicação em OpenStack com Setup Automatizado

## Objetivo
O objetivo deste roteiro é implantar uma aplicação utilizando o OpenStack, aproveitando a infraestrutura bare-metal criada previamente no [Roteiro 1](../roteiro1/main/). O processo abrange o setup do OpenStack, configuração de imagens, redes, flavors e a distribuição de instâncias de aplicação e banco de dados em máquinas virtuais.

---

## Criação da Infraestrutura
Este roteiro reutiliza a infraestrutura criada no Roteiro 1:

- 5 servidores físicos provisionados pelo MAAS.
- Juju para orquestração.
- Bridge externa `br-ex` criada para conexão com o mundo externo.

---

## Setup do OpenStack

### Tarefa 1 - Validação Inicial

- ![](img/tarefa1_1.png)
- ![](img/tarefa1_2.png)
- ![](img/tarefa1_3.png)
- ![](img/tarefa1_4.png)
- ![](img/tarefa1_5.png)

### Etapas Executadas:

1. **Autenticação:** carregamento das credenciais via `source openrc`
2. **Horizon:** acesso ao dashboard e acompanhamento em tempo real.
3. **OpenStack Client:** instalado via `snap` na máquina main.
4. **Serviços:** verificados com `openstack service list`
5. **Configurações adicionais:**

<!-- bloco de código -->
```bash
juju config neutron-api enable-ml2-dns="true"
juju config neutron-api-plugin-ovn dns-servers="172.16.0.1"
```

6. **Imagem:** Ubuntu Jammy importada manualmente.
7. **Flavors:** criados sem disco efêmera:

- `m1.tiny` (1 vCPU, 1 GB RAM, 20 GB disk)
- `m1.small`, `m1.medium`, `m1.large`

8. **Rede Externa:** criada na faixa `172.16.7.0/20`
9. **Rede Interna:** criada na faixa `192.169.0.0/24`
10. **Roteador:** configurado para conectar rede interna à externa
11. **Security Groups:** SSH e ICMP liberados
12. **Key Pair:** importada `id_rsa.pub` da máquina main
13. **Instância de teste:** `client` disparada com flavor `m1.tiny`
14. **Floating IP:** alocado e testado com sucesso via SSH

### Tarefa 2 - Mudanças no Setup

- ![](img/tarefa2_1.png)
- ![](img/tarefa2_2.png)
- ![](img/tarefa2_3.png)
- ![](img/tarefa2_4.png)

Explicação sobre diferenças e como cada recurso foi criado.

### Tarefa 3 - Escalando os Nós

- Liberação da máquina reserva no MAAS
- Adição de nova unidade do `nova-compute`:

<!-- bloco de código -->
```bash
juju add-unit nova-compute
```

- Adição de unidade para block storage:

<!-- bloco de código -->
```bash
juju add-unit --to <machine-id> ceph-osd
```

- ![](img/tarefa3_1.png)

---

## App: Deploy da Aplicação em VMs OpenStack

### Tarefa: Distribuição da Aplicação

- 2 instâncias com a API do projeto
- 1 instância com o banco de dados
- 1 instância com Nginx (Load Balancer)

### Topologia:

- Rede interna: `192.169.0.0/24`
- Rede externa: `172.16.0.0/20`
- Balanceador distribui requests para APIs

### Tarefa 4 - Relatório e Prints

- ![](img/tarefa4_1.png)
- ![](img/tarefa4_2.png)
- ![](img/tarefa4_3.png)
- ![](img/tarefa4_4.png)
- ![](img/tarefa4_5.png)

---

## Discussões

A parte mais complexa foi o setup inicial do OpenStack, especialmente a configuração de redes e roteadores. A documentação oficial exigiu interpretação cuidadosa para adaptação ao nosso ambiente.

Por outro lado, a utilização do Horizon tornou o processo de gerenciamento muito mais intuitivo. O deploy de instâncias e configuração de key pairs e security groups foram etapas bem sucedidas com impacto direto na funcionalidade do sistema.

O balanceamento via Nginx funcionou conforme esperado, mostrando que o OpenStack fornece uma estrutura robusta e escalável para hospedar aplicações em ambientes reais.

---

## Conclusão

O Roteiro 3 demonstrou a versatilidade do OpenStack como plataforma de virtualização em ambiente bare-metal. Aproveitando a infraestrutura criada anteriormente, conseguimos configurar uma nuvem privada funcional e escalar recursos conforme a demanda.

Com as instâncias distribuídas entre diferentes servidores e a aplicação balanceada via Nginx, atingimos um ambiente semelhante ao de produção em empresas, com boas práticas de separação de serviços e alta disponibilidade.