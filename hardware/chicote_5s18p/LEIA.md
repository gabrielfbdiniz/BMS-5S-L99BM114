# PCB 5s18p, 32 NTC com mux, rev C (auditoria de integracao com o BMS, 30/out)

## Rev C: o que mudou

1. **U3 agora e TPS70950DBVR (5,0 V)**, mesmo SOT-23-5 e mesma pinagem do TPS70933. Os mux (74HC4051) passam a rodar em 5 V, igual ao VCOM (S0-S2) e ao VTREF do L99BM114: sem violar VCC+0,5 V nos pinos de selecao e sem ceifar a leitura do NTC acima de ~3,6 V.
   - Entrada: no mais alto de cada placa. NEG = BATT13 (4 celulas, 10 a 16,8 V), POS = BATT14 (5 celulas, 12,5 a 21 V). Os dois ficam acima de 5 V + dropout mesmo com celula a 2,5 V, e abaixo dos 30 V do CI.
   - C3 (entrada) = 1 uF 50 V X7R. C4 (saida) = 1 uF 16 V X7R.
   - Continua valendo: o AGND do BMS tem que ser o negativo do modulo (GND_ESD), porque o LDO usa o AGND como terra.
2. **J1 diferente em cada placa (anti-inversao)**: NEG continua Micro-Fit 2x05 (43045-1012); POS passou para Micro-Fit 2x06 (43045-1212). A coluna extra da POS (pinos 6 e 12) e NC, e os outros 10 pinos ficaram nas mesmas posicoes, entao o roteamento so mudou em volta do pino-guia novo. Pinagem nova da POS em CHICOTE_BMS.md.
3. **Serigrafia nas duas faces**: "PLACA NEGATIVA - CONEXAO J1_NEG (2x05)" / "PLACA POSITIVA - CONEXAO J1_POS (2x06)" ao longo da faixa do conector, e triangulo cheio no pino 1 do J1.
4. **Fusiveis de sense F1-F3**: Littelfuse 0466.125NRHF (1206, muito rapido, 125 mA, 125 V AC/DC, 50 A de interrupcao). E o valor mais baixo da serie acima de 100 mA; tem 62 mA (0466.062) se o Gabriel quiser mais baixo.
   - Conferir: se o balanceamento passivo do L99BM114 passa por esses fios, a corrente de balanceamento tem que ficar bem abaixo de 125 mA, e a resistencia a frio do fusivel (alguns ohms) entra em serie com a medida durante o balanceamento.
5. **ERC**: flag de No Connect no J1.1 (as duas placas), J1.6/J1.12 (POS) e no pino 4 (NC) do U3.

Verificacao rev C: DRC do KiCad 0 violacoes e 0 pads desconectados nas duas placas; esquematico x placa: 0 divergencias pino a pino; nenhuma curva de trilha acima de 45 graus fora de via.


Duas placas por modulo: NEG (ilhas P1 P3 P5) e POS (ilhas P2 P4 P6). Abra a pasta de cada uma e de dois cliques no .kicad_pro.

Cada pasta tem placa (.kicad_pcb) + esquematico (.kicad_sch) ligados: F8 (Atualizar placa a partir do esquematico) nao muda nada. Footprints da biblioteca padrao do KiCad (R_0805, Fuse_1206, SOIC-16, C_0805, PinHeader 1x10). As celulas estao no esquematico como H1 a H90, uma por furo, cada uma com 1 pino ligado na sua ilha (P1/P3/P5 ou P2/P4/P6), com o Value '+' ou '-' indicando o terminal daquela face. Na placa, cada H e um furo NPTH de 16,5 mm com um anel de cobre so no F.Cu, que e o pad, a 0,3 mm do furo. O anel nao vai para o B.Cu, porque o B.Cu e o plano AGND. Os footprints estao na biblioteca local FormulaITA.pretty, ja configurada no fp-lib-table da pasta.
Os arquivos estao no formato do KiCad 7: abrem normal no 8 e no 9, que pergunta se quer converter ao salvar.

## Historico: rev B.2 / B.3 (substituido pela rev C onde conflita) (ajustes do Gabriel, 27 e 28/out)

- Conector de cada placa trocado pelo Molex Micro-Fit 3.0 2x05 (43045-1012), com trava positiva. As duas placas vao para um unico Micro-Fit 2x08 no BMS pelo chicote em Y descrito em CHICOTE_BMS.md.
- Sem pontas nas trilhas: nenhuma curva passa de 45 graus. Nos cruzamentos dos canais o barramento faz um trecho reto curto, e as outras curvas viraram chanfro de 45 graus. Conferido no arquivo final: 0 cantos acima de 45 graus.
- Alimentacao do mux (rev B.3): na propria placa, sem fio pro BMS. U3 = LDO TPS70933 (3,3 V, entrada ate 30 V, consumo de 1 uA) com 1 uF na entrada (50 V) e 1 uF na saida.
  - Entrada: o no mais alto da placa, ja protegido pelo fusivel de sense (NEG: BATT13, 4 celulas, ate 16,8 V; POS: BATT14, 5 celulas, ate 21 V). Terra: AGND.
  - Isso so funciona se o AGND do BMS for o negativo do modulo (o mesmo potencial de GND_ESD/P1). Confirmar com o Gabriel.
  - O sinal do NTC (divisor do pull-up do BMS) nao pode passar da alimentacao do mux: o pull-up (VTREF) tem que ser <= 3,3 V, e os pinos MUX_S0..S2 tem que ser logica de 3,3 V.
  - O consumo (uns poucos uA) sai das celulas de baixo do modulo. E desprezivel perto da autodescarga, mas e um consumo permanente.
  - J1.1 ficou livre.
  - Regra extra no .kicad_dru: dentro do U3 (SOT-23-5, passo 0,95) a folga entre a entrada (ate 21 V) e o GND e 0,3 mm. A IPC-2221 pede 0,13 mm ate 30 V.

## Modelos 3D

NTCs, fusiveis, mux, capacitores e conector usam os footprints da biblioteca padrao com o modelo 3D apontando para ${KICAD10_3DMODEL_DIR} (KiCad 10). Se abrir no KiCad 9, troque por ${KICAD9_3DMODEL_DIR} em Preferencias > Configurar caminhos, ou atualize os footprints pela biblioteca. As celulas nao tem modelo 3D (a biblioteca do KiCad nao tem 21700).

## O que mudou em relacao a rev A

1. **Trilhas**
   - Nas celulas, as trilhas correm em barramentos paralelos pelos canais retos entre os furos: segmentos de 30 graus, espacamento constante, sem curvas soltas.
   - Na faixa do conector, o roteamento e ortogonal: horizontais no B.Cu, verticais no F.Cu e troca de camada por via. Nao tem via dentro de pad nem debaixo do corpo dos componentes.
2. **Plano AGND macico no B.Cu**
   - Cobre a placa inteira. Os NTCs, os mux, os capacitores e o pino 2 do J1 ligam direto no plano. Nao existe mais nenhuma trilha de terra.
   - O plano ja vem preenchido. Se mexer em algo, aperte B para preencher de novo.
3. **NTCs**
   - Ficam deitados, encostados na borda do furo da celula que medem: o centro do NTC fica a 9,3 mm do centro da celula (antes eram 12,1 mm). Isso da mais folga para os 10 mm da EV.7.5.4.
   - Ficam no lado livre do cruzamento dos canais, fora do caminho do barramento.
   - Padrao regular por grupo: 4+4 por coluna nos grupos com 8, 3+3 no G3 e 3+2 no G1 e no G5. A tabela em PINAGEM_e_NTCs.md diz qual celula cada NTC mede.
   - Fusiveis de sense: mesmo esquema dos NTCs, com via direto na ilha.

## Verificacao

- DRC oficial do KiCad (7.0.11) com as regras do .kicad_dru: **0 violacoes e 0 pads desconectados** nas duas placas. Os relatorios sao os arquivos *_DRC.rpt.
- Regras de espacamento:

  | Entre | Distancia minima |
  |---|---|
  | Ilhas | 2,5 mm |
  | Potencia (ilhas, fusiveis, BATTx) e o resto | 0,5 mm |
  | Sinal e sinal | 0,2 mm |
  | Cobre e furo da celula | 0,25 mm |
  | Cobre e borda da placa | 0,3 mm |

- Furos de fixacao com 7 mm sem cobre nas duas camadas.

## Pendencias

- Confirmar que a placa do lado do Busbar_unico_esq e a NEG.
- O suporte das celulas precisa de rebaixo para os NTCs, os fusiveis e os mux no B.Cu.
