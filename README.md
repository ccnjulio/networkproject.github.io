## Introdução ao Projeto.
Este projeto consiste na simulação de uma infraestrutura de rede corporativa, projetada para garantir segmentação, organização lógica e redundância de conectividade. A rede é estruturada com VLANs distintas para diferentes setores, otimizando o desempenho e a segurança da comunicação interna.

A infraestrutura conta com switches de Camada 2 e Camada 3, roteadores interligados e um conjunto de servidores essenciais, incluindo DNS, DHCP e HTTP. Além disso, mecanismos de redundância foram implementados para garantir disponibilidade e continuidade operacional em caso de falhas na rede.

## Infraestrutura.
A rede foi projetada para simular um ambiente corporativo robusto, utilizando o Cisco Packet Tracer como ferramenta de modelagem e configuração. A topologia implementa segmentação por VLANs (10, 20, 30, 40, 50, 60) para otimizar o tráfego e reforçar a segurança, enquanto switches de Camada 2 e Camada 3 garantem a conectividade interna e o roteamento entre redes.

Três roteadores interligam a infraestrutura, proporcionando redundância e continuidade operacional. Além disso, servidores DNS, DHCP e HTTP foram configurados para gerenciar atribuição dinâmica de endereços IP, resolução de nomes e hospedagem de aplicações web.

A configuração inclui protocolos de roteamento dinâmico e distribuição eficiente do tráfego, garantindo desempenho e disponibilidade para os usuários da rede.

> Abra a imagem e uma nova aba para exibir com melhor resolução.
![Branching](assets/INFRAESTRUTURA.png)

## Segmentação de VLANs

VLAN 10 - WiFi
  Exclusiva para dispositivos sem fio, garantindo isolamento e melhor gerenciamento do tráfego. Todos os dispositivos conectados ao Access Point, como notebooks, smartphones e impressoras Wi-Fi, operam dentro desse segmento. A atribuição de endereços IP é gerenciada 
dinamicamente por um servidor DHCP.

VLAN 20 - TI
  Segmento exclusivo para o setor de Tecnologia da Informação, garantindo um ambiente isolado e seguro para administradores de rede e técnicos. A comunicação desta VLAN com redes externas é restrita, limitando acessos não autorizados e reforçando a segurança dos dispositivos críticos.
    
VLAN 30 - RH
  Destinada à segmentação de dispositivos específicos para o ambiente de Recursos Humanos, incluindo estações de trabalho (PCs) e impressoras, que atuam como endpoints dentro desta VLAN.
    
VLAN 40 - Vendas
  Esta VLAN é alocada ao setor de Vendas, seguindo a mesma estrutura da VLAN de Recursos Humanos. A VLAN 40 contém dispositivos como PCs e impressoras, que funcionam como endpoints para atender às necessidades específicas do setor.
    
VLAN 50 - Treinamento
  Dedicada ao processo de treinamento (Onboarding) de novos colaboradores. Nela, são disponibilizados os endpoints necessários para apoiar o desenvolvimento profissional dos funcionários.
    
VLAN 60 - Convidados
  Esta VLAN é dedicada ao tráfego de convidados, sendo isolada das demais VLANs da rede para prevenir possíveis ações indesejadas e garantir a segurança da infraestrutura.

## Switches de Acesso (Layer 2)

  Os switches de acesso, SW-ACESSO1 e SW-ACESSO2, foram implementados para atuar como pontos de conexão entre os dispositivos finais (endpoints) e a infraestrutura de rede principal. Eles desempenham um papel essencial na distribuição do tráfego, garantindo que os pacotes sejam encaminhados corretamente dentro das VLANs definidas.

### Switch de Acesso 1 (SW-ACESSO1) ###
    
  Para configurar as portas FastEthernet 1 a 8 como membros da VLAN 10, foram executados os seguintes comandos:
    
    interface range fastEthernet0/1-8
    switchport access vlan 10

  Do mesmo jeito, foram efetuados os comandos:

    interface range fastEthernet0/9-16
    switchport access vlan 20
  
    interface range fastEthernet17-24
    switchport access vlan 30


### Switch de Acesso 2 (SW-ACESSO2) ###
  
  Para configurar as VLANS 40, 50 e 60, foram efetuados os mesmos comandos acima, mas com seus respectivos números:
      
      interface range fastEthernet0/1-8
      switchport access vlan 40
      
      interface range fastEthernet0/9-16
      switchport access vlan 50
      
      interface range fastEthernet0/17-24
      switchport access vlan 60


Também foi essencial a configuração das interfaces de uplink no modo trunk, permitindo o tráfego de múltiplas VLANs entre os switches de acesso, switches de distribuição e roteadores. O modo trunk garante que os quadros Ethernet sejam corretamente identificados e encaminhados para suas respectivas VLANs, possibilitando a comunicação eficiente entre os diferentes segmentos da rede.

Na porta, os comandos utilizados:
  
    switchport mode trunk
    switchport trunk allowed vlan 10,20,30 <--- Para o SW-ACESSO1
  

    switchport mode trunk
    switchport trunk allowed vlan 40,50,60 <--- Para o SW-ACESSO2

## Switches de Distribuição DIST-1 e DIST-2 (Layer 3)

Os switches multilayer, DIST-1 e DIST-2, foram implementados para atuar como a camada de distribuição da rede, estabelecendo a comunicação entre os switches de acesso e os roteadores. Diferente dos switches de camada 2, esses dispositivos possuem capacidade de roteamento, permitindo que interfaces sejam configuradas como portas roteáveis. Isso viabiliza a interconexão entre VLANs e a comunicação com os serviços essenciais da rede, como DHCP, DNS e HTTP.

Foram configuradas seis VLANs (10, 20, 30, 40, 50, 60) e atribuídos nomes específicos para cada uma, garantindo uma segmentação lógica eficiente da rede. Cada VLAN recebeu um endereço IP estático (192.168.10.0/24, 192.168.20.0/24, ..., 192.168.60.0/24) para atuar como gateway padrão dos dispositivos pertencentes ao respectivo segmento. Além disso, foi configurado um IP Helper-Address em cada interface de VLAN, permitindo que as requisições DHCP dos dispositivos fossem encaminhadas corretamente ao servidor DHCP centralizado. Essa configuração assegura a atribuição automática de endereços IP dentro de cada VLAN, facilitando a gestão e distribuição de endereços na rede.

Comandos executados:

    interface vlan 10
    ip address 192.168.10.0 255.255.255.0
    ip helper-address 192.168.100.5 <---- IP do servidor DHCP

>Esse processo foi repetido para cada VLAN, ajustando os valores específicos de endereço IP, máscara de sub-rede e IP Helper-Address conforme necessário. Dessa forma, cada segmento de rede recebeu a configuração adequada para garantir roteamento eficiente, segmentação lógica e atribuição dinâmica de endereços IP via DHCP.

Foi implementado o protocolo de roteamento RIP (Routing Information Protocol) para registrar e distribuir automaticamente as rotas das redes conhecidas entre os dispositivos de Camada 3. Essa configuração permite que os roteadores e switches multilayer compartilhem informações sobre suas redes vizinhas, garantindo a conectividade entre os diferentes segmentos da infraestrutura.

## Roteadores (ROUTER1, ROUTER2, ROUTER3)

A configuração dos roteadores foi uma das etapas mais simples do projeto. Cada roteador recebeu endereços IP estáticos para garantir conectividade confiável entre os dispositivos de rede.

Além disso, assim como os switches Layer 3, os roteadores tiveram suas redes conhecidas registradas no protocolo RIP, permitindo a troca automática de informações de roteamento. As redes envolvidas na comunicação entre os roteadores foram:

    200.200.100.0/24
    200.200.110.0/24
    200.100.100.0/24
    200.100.110.0/24
    200.100.120.0/24
    192.168.100.0/24 <---- Rede do DHCP
    10.0.0.0/8 <---------- Rede do HTTP

Essa abordagem garantiu um roteamento dinâmico eficiente e a correta propagação das rotas entre os dispositivos de Camada 3 da infraestrutura.

## Servidores (DHCP, DNS, FTP)


  Os servidores foram hospedados em uma rede dedicada, simulando um ambiente isolado semelhante a uma sala de servidores dentro de uma infraestrutura corporativa. Essa segmentação melhora a organização, segurança e gerenciamento dos serviços essenciais da rede.

  Os IPs reservados para os servidores foram:

    Servidor DNS: 192.168.100.4
    Servidor DHCP: 192.168.100.5 <--- IP helper-address
    Servidor FTP: 192.168.100.3

  ![Branching](assets/Servidores.png)

  ## DNS ##
  
Foi configurado um único domínio para fins de teste no servidor DNS, utilizando o nome de domínio 'site.com', associado ao endereço IP '10.0.0.5'.
  
  ![Branching](assets/DNS.png)


  ## DHCP ##

Foram configurados pools de endereços IP para cada rede distribuída nas VLANs. Cada pool incluía as seguintes configurações:

    Gateway padrão (Default Gateway)
    Servidor DNS (DNS Server)
    Endereço inicial do pool (Start IP Address)
    Máscara de sub-rede (Subnet Mask)
    Número máximo de usuários (Max Users)

  ![Branching](assets/DHCP.png)

  ## FTP ##

>O servidor não foi completamente configurado, sendo utilizado apenas como um ambiente de demonstração.


  ## HTTP ##

A configuração da rede foi realizada para viabilizar a hospedagem do servidor web acessível através do endereço http://site.com.

    Servidor HTTP: 10.0.0.5

  ![Branching](assets/http.png)

  ## Teste de Conexão ##

Os testes foram conduzidos com o seguinte objetivo: assegurar a comunicação entre os endpoints e os servidores por meio de pacotes ICMP (ping) em ambos os sentidos.

PC VEN1 ---> http://site.com

  ![Branching](assets/Teste1.png)


  ## Conclusão ##

Por meio deste projeto, foi possível aprofundar e aplicar na prática conceitos fundamentais de redes, como a segmentação em VLANs, roteamento inter-VLAN, protocolos de redundância, conectividade entre sub-redes, distribuição de serviços, configuração de dispositivos de camada 2 (Switches) e camada 3 (Roteadores), além da implementação de serviços como DHCP e DNS. A experiência também envolveu o gerenciamento de tabelas de roteamento, trunking e a otimização do tráfego para garantir desempenho e segurança.
  
  

  


