# ADR 001 - Idempotência em pagamentos PIX

**Status:** Aceita
**Data:** 2026-10-12

## Contexto
PIX duplicado pode gerar pagamento em dobro e processo por cobrança indevida.

## Decisão
Implementar chave de idempotência `idempotency_key` em toda requisição de pagamento. Guardar por 24h no Redis. Se mesma chave chegar, retornar resultado anterior sem processar de novo.

## Consequências
Evita duplicidade, facilita auditoria BACEN, garante estorno automático.
