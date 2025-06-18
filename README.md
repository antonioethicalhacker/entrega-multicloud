# Passo a Passo: Criação de Conexão Multi-Cloud AWS e Azure

## Configuração na Azure

1. Crie um **Resource Group** na região desejada.
2. Crie uma **VNET**:
   - Exemplo: `192.168.0.0/16`
3. Crie as sub-redes:
   - Sub-rede privada: `192.168.1.0/24`
   - Sub-rede para o gateway: `192.168.2.224/27`
4. Crie uma **Virtual Network Gateway** na mesma região do Resource Group:
   - Tipo de gateway: `VPN`
   - Geração: `1`
5. Crie **dois IPs públicos**:
   - Tipo: `BASIC`
   - Nome: pode ser qualquer um.
6. Configurações:
   - Sem BGP
   - Tipo: Dinâmico

---

## Configuração na AWS

1. Crie uma **VPC** na região desejada:
   - Exemplo: `172.16.0.0/16`
2. Crie a **sub-rede privada**:
   - Exemplo: `172.16.1.0/24`
3. Crie um **Customer Gateway**:
   - Utilize o IP do Virtual Network Gateway criado na Azure.
4. Crie um **Virtual Private Gateway** e associe-o à sua VPC.
5. Crie uma **Site-to-Site VPN Connection**:
   - Selecione a VPC criada.
   - Selecione o Customer Gateway criado.
   - Selecione o Virtual Private Gateway criado.
   - Selecione a opção de prefixo estático e insira o IP da sub-rede privada da Azure.

6. Após criar, **baixe o arquivo de configuração** da conexão:
   - Formato: `Generic`, `Generic`, `IKEv2`

---

## Configuração Final na Azure

1. Crie um **Local Network Gateway**:
   - IP: Utilize o IP da conexão do túnel 1 (Outside IP da AWS)
   - Prefixo: Utilize o IP da VPC da AWS.
   
2. Opcional: Crie um segundo Local Network Gateway para o túnel 2, utilizando o segundo Outside IP.

3. Aguarde o status da conexão ficar como **"Conectado"**.

---

## Finalização na AWS

1. Crie uma **tabela de rotas**.
2. Crie e associe um **Internet Gateway** à sua VPC.
3. Configure a tabela de rotas:
   - `0.0.0.0/0` → Internet Gateway
   - `192.168.1.0/24` → Virtual Private Gateway (VPG)

---

## Testes

- Verifique a conectividade entre as VMs das duas clouds.
- Faça testes de ping ou conexão entre as redes.

---

## Se der erro:

- Vá até a **Site-to-Site VPN Connection** na AWS.
- Edite a configuração e insira novamente o IP da sub-rede privada da Azure como prefixo estático.

---

## Observações

- Certifique-se de que os **Security Groups e Network Security Groups (NSG)** estejam liberando o tráfego entre as redes.
- Mantenha o protocolo **IKEv2** configurado em ambos os lados.
- O tempo de propagação da VPN pode variar entre alguns minutos.
- Adicione uma sub-rede pública em cada uma para possibilitar o acesso remoto (SSH).
---

> ✅ Conexão Multi-Cloud finalizada com sucesso!

Prints de cada passo (Sem descrição):
https://drive.google.com/file/d/1uMZrL7AgQ22V7U_MzLPWLtmJHrFEJapX/view?usp=drive_link

Topologia:
https://drive.google.com/file/d/1_h6vzPyscRFLjVKeb4veOxCk8L9tShYm/view?usp=drive_link
