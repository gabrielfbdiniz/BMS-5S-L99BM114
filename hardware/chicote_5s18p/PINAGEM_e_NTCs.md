# Pinagem e posicao dos NTCs (rev C)

Coluna 1 = a do lado do conector. Celula contada de baixo (lado y maior) pra cima, vista pela face F.Cu.

## Placa NEG

Mux U1 em y = 54.86 mm, U2 em y = 152.33 mm (B.Cu, x = 17 mm).

### Conector J1 (Molex Micro-Fit 3.0 2x05, 43045-1012) -> chicote -> J3 do BMS (Micro-Fit 2x08)

| Pino J1 | Sinal | Pino no J3 | Funcao |
|---|---|---|---|
| J1.1 | NC | - | livre, flag de No Connect (mux alimentado na propria placa pelo LDO U3, 5 V) |
| J1.2 | MUX_S0 | 11 | selecao bit 0 (os dois mux) |
| J1.3 | MUX_S1 | 10 | selecao bit 1 |
| J1.4 | MUX_S2 | 9 | selecao bit 2 |
| J1.5 | AGND | 12 | plano AGND (referencia dos NTCs e GND dos mux) |
| J1.6 | NTC_CH1 | 16 | saida do mux (NTC) |
| J1.7 | NTC_CH2 | 15 | saida do mux (NTC) |
| J1.8 | BATT2 | 4 | polo (+) da celula (via fusivel) |
| J1.9 | BATT13 | 2 | polo (+) da celula (via fusivel) |
| J1.10 | GND_ESD | 6 | polo (-) da celula 1 (via fusivel) |

### U1 -> NTC_CH1 (canal = S2 S1 S0)

| Canal | NTC | Celula medida |
|---|---|---|
| Y0 (000) | RT10 | grupo G3, coluna 6, celula 7 de baixo pra cima |
| Y1 (001) | RT14 | grupo G5, coluna 1, celula 6 de baixo pra cima |
| Y2 (010) | RT16 | grupo G5, coluna 2, celula 9 de baixo pra cima |
| Y3 (011) | RT15 | grupo G5, coluna 2, celula 5 de baixo pra cima |
| Y4 (100) | RT11 | grupo G3, coluna 6, celula 9 de baixo pra cima |
| Y5 (101) | RT8 | grupo G3, coluna 5, celula 6 de baixo pra cima |
| Y6 (110) | RT5 | grupo G1, coluna 10, celula 9 de baixo pra cima |
| Y7 (111) | RT3 | grupo G1, coluna 9, celula 6 de baixo pra cima |

### U2 -> NTC_CH2 (canal = S2 S1 S0)

| Canal | NTC | Celula medida |
|---|---|---|
| Y0 (000) | RT6 | grupo G3, coluna 5, celula 1 de baixo pra cima |
| Y1 (001) | RT1 | grupo G1, coluna 9, celula 1 de baixo pra cima |
| Y2 (010) | RT4 | grupo G1, coluna 10, celula 5 de baixo pra cima |
| Y3 (011) | RT12 | grupo G5, coluna 1, celula 1 de baixo pra cima |
| Y4 (100) | RT2 | grupo G1, coluna 9, celula 3 de baixo pra cima |
| Y5 (101) | RT9 | grupo G3, coluna 6, celula 4 de baixo pra cima |
| Y6 (110) | RT7 | grupo G3, coluna 5, celula 3 de baixo pra cima |
| Y7 (111) | RT13 | grupo G5, coluna 1, celula 3 de baixo pra cima |

## Placa POS

Mux U1 em y = 77.67 mm, U2 em y = 150.83 mm (B.Cu, x = 17 mm).

### Conector J1 (Molex Micro-Fit 3.0 2x06, 43045-1212) -> chicote -> J3 do BMS (Micro-Fit 2x08)

| Pino J1 | Sinal | Pino no J3 | Funcao |
|---|---|---|---|
| J1.1 | NC | - | livre, flag de No Connect (mux alimentado na propria placa pelo LDO U3, 5 V) |
| J1.2 | MUX_S0 | 11 | selecao bit 0 (os dois mux) |
| J1.3 | MUX_S1 | 10 | selecao bit 1 |
| J1.4 | MUX_S2 | 9 | selecao bit 2 |
| J1.5 | AGND | 12 | plano AGND (referencia dos NTCs e GND dos mux) |
| J1.6 | NC | - | livre, flag de No Connect (mux alimentado na propria placa pelo LDO U3, 5 V) |
| J1.7 | NTC_CH3 | 14 | saida do mux (NTC) |
| J1.8 | NTC_CH4 | 13 | saida do mux (NTC) |
| J1.9 | BATT12 | 3 | polo (+) da celula (via fusivel) |
| J1.10 | BATT1 | 5 | polo (+) da celula (via fusivel) |
| J1.11 | BATT14 | 1 | polo (+) da celula (via fusivel) |
| J1.12 | NC | - | livre, flag de No Connect (mux alimentado na propria placa pelo LDO U3, 5 V) |

### U1 -> NTC_CH3 (canal = S2 S1 S0)

| Canal | NTC | Celula medida |
|---|---|---|
| Y0 (000) | RT3 | grupo G2, coluna 3, celula 6 de baixo pra cima |
| Y1 (001) | RT8 | grupo G2, coluna 4, celula 7 de baixo pra cima |
| Y2 (010) | RT16 | grupo G4, coluna 8, celula 7 de baixo pra cima |
| Y3 (011) | RT11 | grupo G4, coluna 7, celula 6 de baixo pra cima |
| Y4 (100) | RT4 | grupo G2, coluna 3, celula 8 de baixo pra cima |
| Y5 (101) | RT7 | grupo G2, coluna 4, celula 5 de baixo pra cima |
| Y6 (110) | RT12 | grupo G4, coluna 7, celula 8 de baixo pra cima |
| Y7 (111) | RT15 | grupo G4, coluna 8, celula 5 de baixo pra cima |

### U2 -> NTC_CH4 (canal = S2 S1 S0)

| Canal | NTC | Celula medida |
|---|---|---|
| Y0 (000) | RT1 | grupo G2, coluna 3, celula 3 de baixo pra cima |
| Y1 (001) | RT6 | grupo G2, coluna 4, celula 4 de baixo pra cima |
| Y2 (010) | RT14 | grupo G4, coluna 8, celula 4 de baixo pra cima |
| Y3 (011) | RT9 | grupo G4, coluna 7, celula 3 de baixo pra cima |
| Y4 (100) | RT2 | grupo G2, coluna 3, celula 5 de baixo pra cima |
| Y5 (101) | RT5 | grupo G2, coluna 4, celula 2 de baixo pra cima |
| Y6 (110) | RT10 | grupo G4, coluna 7, celula 5 de baixo pra cima |
| Y7 (111) | RT13 | grupo G4, coluna 8, celula 2 de baixo pra cima |
