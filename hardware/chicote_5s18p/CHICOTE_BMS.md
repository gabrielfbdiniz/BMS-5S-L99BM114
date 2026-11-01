# Chicote: placas NEG + POS -> J3 do BMS (1 conector so, 16 vias) - rev C

Conectores (todos Molex Micro-Fit 3.0, trava positiva):
- Placa NEG, J1: 43045-1012 (2x05 vertical). Plug do chicote: 43025-1000 (receptaculo 2x05). Carcaca PRETA.
- Placa POS, J1: 43045-1212 (2x06 vertical). Plug do chicote: 43025-1200 (receptaculo 2x06). Usar carcaca de COR DIFERENTE (ou marcar com fita/termocontratil vermelho).
- BMS, J3: 2x08. Plug do chicote: 43025-1600.
- Terminais: femea da serie 43030, pela bitola do fio.

Anti-inversao: o plug 2x06 da POS nao entra no J1 2x05 da NEG. O plug 2x05 da NEG e menor que o J1 2x06 da POS e pode chegar a entrar nele: conferir com uma peca fisica e, por garantia, manter cor diferente + serigrafia "PLACA POSITIVA - CONEXAO J1_POS" / "PLACA NEGATIVA - CONEXAO J1_NEG" (as duas faces) e o triangulo do pino 1.
Na POS os pinos 6 e 12 (coluna extra) ficam sem fio. Os outros 10 pinos ocupam as mesmas posicoes fisicas dos 10 da NEG.

Os sinais comuns (MUX_S0, S1, S2 e AGND) bifurcam em Y perto do J3 (emenda com termocontratil), um fio so ate cada terminal do J3.

| J3 (BMS) | Sinal | Vem de |
|---|---|---|
| 1 | BATT14 | POS J1.11 |
| 2 | BATT13 | NEG J1.9 |
| 3 | BATT12 | POS J1.9 |
| 4 | BATT2 | NEG J1.8 |
| 5 | BATT1 | POS J1.10 |
| 6 | GND_ESD | NEG J1.10 |
| 7 | livre | - |
| 8 | livre | - |
| 9 | MUX_S2 | Y: NEG J1.4 + POS J1.4 |
| 10 | MUX_S1 | Y: NEG J1.3 + POS J1.3 |
| 11 | MUX_S0 | Y: NEG J1.2 + POS J1.2 |
| 12 | AGND | Y: NEG J1.5 + POS J1.5 |
| 13 | NTC_CH4 | POS J1.8 |
| 14 | NTC_CH3 | POS J1.7 |
| 15 | NTC_CH2 | NEG J1.7 |
| 16 | NTC_CH1 | NEG J1.6 |

Atencao: na POS a numeracao mudou por causa do 2x06 (NTC_CH3 = J1.7, NTC_CH4 = J1.8, BATT12 = J1.9, BATT1 = J1.10, BATT14 = J1.11). A NEG nao mudou.

Os fios BATT/GND_ESD sao do sistema de tracao. Isolacao adequada e chicote preso, sem esforco nos conectores (EV.6.5.6).
