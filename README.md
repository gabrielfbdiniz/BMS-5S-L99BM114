# BMS 5S (L99BM114) — Formula ITA
### Módulo Escravo Distribuído de Sensoriamento de Células, Balanceamento e Termistores NTC (Formula SAE EV)

Repositório oficial de **Hardware** e **Documentação Técnica** do módulo escravo (Slave) do Battery Management System (BMS) da equipe **Formula ITA** (Temporada 2025-2026).

---

## Renderizações 3D do Hardware

### Módulo BMS 5S Slave (L99BM114)

Placa de 4 camadas dedicada ao monitoramento de cada bloco 5S, dotada de barreira galvânica de isolação automotiva, balanceamento passivo e filtros diferenciais de alta precisão.

<div align="center">
  <img src="renders/slave_render_iso_top.png" alt="BMS 5S Slave - Perspectiva Superior" width="850px" />
  <p><em>Vista Isométrica Superior — Destaque para o ASIC ST L99BM114, transformadores de pulso Pulse HMU2103NL (Isolated SPI Daisy-Chain), filtros de entrada e conectores Molex Micro-Fit 3.0.</em></p>
</div>

<br>

| Perspectiva Superior (Iso Top) | Perspectiva Inferior (Iso Bottom) |
| :---: | :---: |
| ![Top Iso](renders/slave_render_iso_top.png) | ![Bottom Iso](renders/slave_render_iso_bottom.png) |
| **Vista Ortográfica Superior (Top View)** | **Vista Ortográfica Inferior (Bottom View)** |
| ![Top Ortho](renders/slave_render_top.png) | ![Bottom Ortho](renders/slave_render_bottom.png) |

---

## Destaques de Arquitetura e Engenharia

1. **Topologia do Sistema no Acumulador:**
   * O acumulador total de tração é composto por **8 módulos 5s18p** em série (**40S18P = 720 células** formato 21700, 168V nominais).
   * Cada módulo possui **1x Placa BMS 5S Slave**, comunicando-se em cadeia (daisy-chain) com o BMS Master.
2. **ASIC Automotivo de Alta Resolução (ST L99BM114):**
   * Medição síncrona de tensão de células com ADC de 16 bits ($\pm 1{,}5\text{ mV}$).
   * Balanceamento passivo programável por PWM individual com resistores de potência no layer inferior.
3. **Sensoriamento Térmico Abrangente (FSAE EV4.4):**
   * Interface multiplexada para leitura de termistores NTC 10k 1% por módulo, atendendo aos requisitos de amostragem térmica de células do regulamento FSAE.
4. **Proteção Intrínseca e Filtragem EMI (FSAE EV4.4):**
   * Linhas de medição de tensão protegidas por resistores limitadores de corrente série (`R8, R11, R12, R15, R16` de 100 $\Omega$, 0805) e Ferrite Beads `TDK MMZ2012S102AT000` (1 k$\Omega$ @ 100 MHz, 0805), limitando correntes de curto-circuito em conformidade com o regulamento e atenuando transientes EMI do inversor trifásico.
   * Diodo TVS automotivo `SMA6T68AY` e capacitores cerâmicos de desacoplamento ESD (100 nF / 100 V X7R) em todas as entradas de sensoriamento.
5. **Comunicação Segura em Daisy-Chain (FSAE EV4.8 / IEC 60664):**
   * Barramento Iso-SPI diferencial serial acoplado via transformadores `Pulse HMU2103NL` ($\ge 2500\text{ V}_{RMS}$, creepage $\ge 4{,}0\text{ mm}$ e folga no layout $\ge 6{,}5\text{ mm}$).

---

## Mapeamento de Pinos do Conector de Sensoriamento (J3)

O conector **J3** (Molex Micro-Fit 3.0 de 16 vias — `43045-1600`) é a interface primária do BMS Slave para leitura das tensões de célula, linhas Kelvin de corrente e termistores NTC multiplexados:

| Pino J3 | Sinal | Descrição Técnica |
|:---:|:---:|:---|
| **1** | `BATT14` | Tensão Célula 5 (Topo do módulo, até 21.0V) — Mapeado em Cell 14 do L99BM114 |
| **2** | `BATT13` | Tensão Célula 4 (Até 16.8V) — Mapeado em Cell 13 do L99BM114 |
| **3** | `BATT12` | Tensão Célula 3 (Até 12.6V) — Mapeado em Cell 12 do L99BM114 |
| **4** | `BATT2`  | Tensão Célula 2 (Até 8.4V) — Mapeado em Cell 2 do L99BM114 |
| **5** | `BATT1`  | Tensão Célula 1 (Até 4.2V) — Mapeado em Cell 1 do L99BM114 |
| **6** | `GND_ESD`| Retorno / Referência Negativa do Módulo (0V local) |
| **7** | `ISENSEM_conn` | Entrada Kelvin negativa de sensoriamento de corrente (remoto) |
| **8** | `ISENSEP_conn` | Entrada Kelvin positiva de sensoriamento de corrente (remoto) |
| **9** | `MUX_S2` | Linha de controle digital: Bit 2 dos multiplexadores de NTC |
| **10**| `MUX_S1` | Linha de controle digital: Bit 1 dos multiplexadores de NTC |
| **11**| `MUX_S0` | Linha de controle digital: Bit 0 dos multiplexadores de NTC |
| **12**| `AGND`   | Referência analógica de sensoriamento NTC |
| **13**| `NTC_CH4`| Canal analógico multiplexado 4 (NTCs 25 a 32) |
| **14**| `NTC_CH3`| Canal analógico multiplexado 3 (NTCs 17 a 24) |
| **15**| `NTC_CH2`| Canal analógico multiplexado 2 (NTCs 9 a 16) |
| **16**| `NTC_CH1`| Canal analógico multiplexado 1 (NTCs 1 a 8) |

---

## Topologia de Células, Balanceamento e Componentes DNP (Do Not Populate)

### Topologia 5S no L99BM114
O L99BM114 é um AFE automotivo configurável de 4 a 14 células em série. Conforme a Seção 6.8 do datasheet da STMicroelectronics, as células externas (`Cell 1, 2, 13 e 14`) são mandatórias. Na topologia 5S deste projeto:
- **Células Ativas Monitoradas e Balanceadas:** `Cell 1`, `Cell 2`, `Cell 12`, `Cell 13` e `Cell 14`.
- **Células Não Utilizadas (Cell 3 a 11):** Os pinos de entrada analógica correspondentes estão interligados internamente ao barramento `/NC2` (pinos 32 a 50) e acoplados a `B2_1` através do resistor `R5 (1 kΩ)`, garantindo estabilidade do conversor ADC e impedindo nós flutuantes.

### Tabela de Componentes DNP (NÃO MONTAR)
Os seguintes componentes estão definidos formalmente como **DNP** na montagem:
| Componente | Função Original | Motivo do DNP no Módulo Slave |
| :---: | :---: | :--- |
| **`R17`** (39 $\Omega$, 2010) | Balanceamento de Cell 11 (`S11`) | Canal de célula inexistente na topologia 5S. Não montar para evitar polarização indevida do barramento `/NC2`. |
| **`R26, R27`** (100 $\Omega$, 2512) | Filtro de medição de corrente | O shunt de corrente central de 500A está alocado no módulo Master. Entradas `CURR_P` e `CURR_M` aterradas em `AGND`. |
| **`C23, C24, C25, C26, C27`** | Capacitores de filtro de corrente | DNP conjunto com o bloco de sensoriamento de corrente. |
| **`R49, R50`** (0 $\Omega$, 0805) | Seleção de referência de corrente | DNP conjunto com o bloco de corrente. |

---

## Diretrizes de Montagem e Soldagem SMT (Assembly Guidelines)

1. **Dissipação Térmica do Pad Exposto (IC1 - L99BM114-TR):**
   - O CI L99BM114 possui um Pad Térmico Exposto central (Pad 65, 5.305 x 5.305 mm) conectado a uma **matriz 3x3 de vias térmicas (diâmetro 0.30 mm, anel 0.60 mm)** interligando as camadas `F.Cu`, `In1.Cu`, `In2.Cu` e `B.Cu` ao plano `AGND`.
   - **Stencil do Pad Térmico:** Utilizar abertura de stencil fragmentada (janelada / *windowed array* de 4 ou 9 janelas) com cobertura de solda de **60% a 70%** da área do pad. Isso evita a formação de vazios gasosos (*voids*) e impede que a pasta de solda escorra excessivamente pelos furos das vias térmicas durante o refluxo.
2. **Perfil de Refluxo (Reflow Profile):**
   - Liga de solda recomendada: **SAC305 (Sn96.5 / Ag3.0 / Cu0.5)** Lead-Free, pasta Tipo 4 ou Tipo 5 (No-Clean).
   - Temperatura de pré-aquecimento: 150 °C a 200 °C (tempo de imersão: 60 a 90 segundos).
   - Temperatura de pico (*Peak Reflow*): **245 °C a 250 °C** (tempo acima do líquidus de 217 °C: 45 a 60 segundos).
3. **Inspeção Pós-Montagem:**
   - Inspecionar visualmente e com microscópio ou Raio-X o alinhamento dos pinos de 0.5 mm de pitch do L99BM114, certificando a ausência de pontes de solda (*solder bridges*) entre pinos adjacentes.
   - Verificar a continuidade e isolação da barreira galvânica de 6.5 mm com megômetro (500 VDC, resistência de isolação $> 100\text{ M}\Omega$).

---

## Estrutura do Repositório

```text
BMS 5S - STM - V2/
├── .gitignore                         # Regras de exclusão Git (KiCad, builds, temporários)
├── README.md                          # Visão geral, especificações e documentação rápida
├── LICENSE                            # Licença de Hardware Aberto (CERN-OHL-S v2)
├── CONTRIBUTING.md                     # Diretrizes de contribuição e engenharia
│
├── docs/                              # Documentação técnica e manuais
│   ├── schematics/                    # Esquemáticos oficiais em PDF vetorial
│   │   └── BMS_5S_Slave_Schematic.pdf # Esquemático hierárquico do BMS Slave V2 (KiCad 10)
│   ├── datasheets/                    # Folhas de dados de componentes críticos (L99BM114)
│   └── MANUAL_OPERACIONAL_E_MONTAGEM_BMS.md # Manual de montagem e testes
│
├── hardware/                          # Projetos de Hardware no KiCad 10
│   └── bms_5s_slave/                  # Projeto KiCad do Módulo BMS 5S Slave
│       ├── BMS_5S_v2.kicad_pro        # Projeto mestre KiCad
│       ├── BMS_5S_v2.kicad_sch        # Esquemático mestre
│       ├── BMS_5S_v2.kicad_pcb        # Layout de 4 camadas (Top/In1/In2/Bot)
│       ├── Cell_Sensing.kicad_sch     # Folha: Sensoriamento e balanceamento de células
│       ├── Current_Sensing.kicad_sch  # Folha: Entradas de derivação Kelvin para corrente
│       ├── Isolated_Communication.kicad_sch # Folha: Barreira Iso-SPI e transformadores
│       ├── NTC_and_GPIO.kicad_sch     # Folha: Condicionamento de termistores e GPIOs
│       ├── Power_Management.kicad_sch # Folha: Aterramento inteligente e alimentação
│       ├── Support_and_Debug.kicad_sch# Folha: Pontos de teste e LEDs de status
│       ├── fp-lib-table, sym-lib-table# Mapeamento local de bibliotecas
│       ├── libraries/                 # Símbolos locais e footprints dos componentes dedicados
│       ├── 3dmodels/                  # Modelos 3D STEP/WRL dos conectores e CIs
│       └── fabrication/               # Pacote de manufatura e montagem SMT
│           ├── gerbers/               # Arquivos Gerber RS-274X, Drills Excellon e ZIP final
│           ├── bom/                   # Listas de materiais (BOM CSV, Mouser, Interactive BOM)
│           └── cpl/                   # Arquivo de coordenadas Pick & Place (pos.csv)
│
└── renders/                           # Renderizações fotorrealistas em alta resolução (KiCad 10)
    ├── slave_render_iso_top.png
    ├── slave_render_iso_bottom.png
    ├── slave_render_top.png
    └── slave_render_bottom.png
```

---

## Como Abrir e Visualizar o Projeto no KiCad 10

1. **BMS 5S Slave:**
   - Abra o **KiCad 10** e carregue o arquivo de projeto:  
     `hardware/bms_5s_slave/BMS_5S_v2.kicad_pro`.
   - Abra o PCB Editor e pressione <kbd>Alt</kbd> + <kbd>3</kbd> para abrir o visualizador 3D com raytracing.
2. **Esquemático Rápido em PDF:**
   - Consulte diretamente [`docs/schematics/BMS_5S_Slave_Schematic.pdf`](docs/schematics/BMS_5S_Slave_Schematic.pdf) sem necessidade de abrir o KiCad.

---

<div align="center">
  <sub>Formula ITA — Equipe de Formula SAE Elétrico do Instituto Tecnológico de Aeronáutica (ITA).</sub>
</div>
