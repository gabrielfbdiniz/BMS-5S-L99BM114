# Guia de Contribuição — BMS-5S-L99BM114 (Formula ITA)

Agradecemos o interesse em contribuir com o desenvolvimento do módulo BMS 5S da equipe de Formula SAE. Para manter a integridade de projeto, segurança automotiva e conformidade estrita com o Regulamento da Formula SAE / Student EV, todas as contribuições devem seguir as regras abaixo.

---

## 1. Regras Mandatórias de Projeto (DRC & Segurança)

Antes de submeter qualquer *Pull Request* (PR) ou realizar *commit* no branch principal:

1. **DRC Rigorosamente Zerado:**
   - Execute a verificação de regras de projeto via CLI ou GUI:
     ```bash
     kicad-cli pcb drc --output drc_report.json --format json --severity-all BMS_5S_v2/BMS_5S_v2.kicad_pcb
     ```
   - O projeto não aceita erros de clearance elétrico, desconexões ou invasão de máscaras de solda.

2. **Integridade da Barreira Galvânica:**
   - A distância de escoamento e isolação (*creepage & clearance*) entre o domínio de alta tensão (`AGND` / bateria) e o domínio de baixa tensão isolada (`GND_EXT`) deve ser mantida contínua em **no mínimo 10.00 mm** em todas as camadas (Top, In1, In2 e Bottom).
   - Nenhuma trilha, plano de terra ou via pode atravessar este *keepout* sem passar pelos transformadores de pulso `T1` e `T2` (HMU2103NL).

3. **Portabilidade de Bibliotecas:**
   - Todos os símbolos e pegadas (*footprints*) devem utilizar referências relativas baseadas na variável `${KIPRJMOD}`.
   - Não referencie bibliotecas globais absolutas que apontem para pastas pessoais fora do repositório.

---

## 2. Convenções de Nomenclatura e Netclasses

- **Nets de Potência e Balanceamento:** `/S1` a `/S14`, `/B1` a `/B14` (espessura de trilha mínima de $0.50\text{ mm}$ e vias redundantes em paralelo).
- **Sensoriamento Analógico:** `/C0` a `/C5`, `NTC1` a `NTC7` (filtrados com RC próximo ao ASIC).
- **Comunicação Isolada:** `ISO_RX_P`, `ISO_RX_N`, `ISO_TX_P`, `ISO_TX_N` (pares diferenciais com impedância controlada de $100\,\Omega$ e plano de referência inferior).

---

## 3. Checklist para Pull Requests (PR)

- [ ] O esquemático compila sem erros no ERC (*Electrical Rules Check*).
- [ ] O layout passa no DRC com 0 violações críticas e 0 desconexões.
- [ ] As zonas de terra (`AGND` e `GND_EXT`) foram recalculadas (*Fill Zones* - Tecla `B`).
- [ ] A pasta de manufatura (`fabrication/`) foi atualizada caso haja alterações de layout.
