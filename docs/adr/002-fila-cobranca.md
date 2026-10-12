# ADR 002 - Fila de Cobrança e Retentativa

**Status:** Aceita
**Data:** 2026-10-12

## Contexto
Boletos e notificações de cobrança falhavam quando o provedor caia. Perdíamos janela de negativação.

## Decisão
Usar fila SQS / RabbitMQ com Dead Letter Queue. Toda cobrança é um job com retentativa exponencial (1min, 5min, 30min). Falha final vai pra DLQ para análise manual.

## Consequências
Garante entrega, evita perda de receita e gera log auditável para BACEN.
