# ADR 003 - Tratamento de Propostas de Quitação para Dívidas Prescritas

**Status:** Aceita
**Data:** 2026-10-12
**Autores:** Time de Cobrança e Jurídico
**Decisores:** Diretoria de Risco

## Contexto

Identificamos um volume de propostas de quitação referentes a dívidas com mais de 5 anos de vencimento, já prescritas nos termos do Art. 206 do Código Civil.

Características atuais:
- Dívidas não constam mais na base do banco originador (cedidas para recuperadoras)
- Prazo para negativação em Serasa/SPC e para cobrança judicial já expirado
- Cliente recebeu proposta de quitação com desconto e recusou por entender que a dívida está prescrita
- Risco de, ao tratar como renegociação padrão, gerar novação da dívida e reativar prazo prescricional e possibilidade de negativação

Precisamos definir um padrão sistêmico e jurídico para não gerar passivo de cobrança indevida.

## Decisão

Adotaremos o tratamento de **Quitação por Liberalidade**, sem reconhecimento de dívida.

1.  **Flag no sistema:** Toda dívida com `data_vencimento > 5 anos` receberá `prescribed = true` e `legally_enforceable = false`.

2.  **Fluxo de oferta:** A proposta enviada ao cliente deve ser registrada como `OFFER_SETTLEMENT_PRESCRIBED` e não como `RENEGOTIATION`.

3.  **Vedação:** É vedada a geração e assinatura de "Termo de Confissão de Dívida" ou "Instrumento de Renegociação" para dívidas prescritas. O único documento permitido é o "Termo de Quitação de Obrigação Prescrita por Liberalidade".

4.  **Contabilização:** Em caso de aceite e pagamento, lançar como `RECEITA_EVENTUAL - RECUPERACAO PRESCRITA` e não como `PAGAMENTO_DE_PARCELA`. Não gera novo contrato ou parcelamento ativo.

5.  **Recusa:** Em caso de recusa (cenário atual), apenas logar `offer_declined_at` com motivo, sem alteração de status ou tentativa de negativação.

## Consequências

### Positivas
- Evita reativação do prazo prescricional (novação)
- Elimina risco de negativação indevida e ações de dano moral
- Permite limpar restrição interna (prejuízo interno) do cliente caso ele opte por pagar, sem criar obrigação legal
- Alinha com entendimento do STJ e Código de Defesa do Consumidor

### Negativas
- Não há possibilidade de cobrança judicial ou negativação caso o cliente não aceite
- Necessidade de ajuste no ERP / motor de cobrança para bloquear automaticamente geração de contrato de renegociação quando `prescribed = true`
- Necessidade de treinamento do time de cobrança terceirizada sobre novo termo

## Regras de Validação

```pseudo
IF divida.anos_atraso >= 5 THEN
  SET divida.prescribed = true
  BLOCK operacao.gerar_confissao_divida
  BLOCK operacao.negativar_serasa
  BLOCK operacao.ajuizar_acao
END IF
