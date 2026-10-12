# ADR 005 - Ledger Imutável e Extrato Auditável

**Status:** Aceita
**Data:** 2026-10-12

## Contexto
Extrato alterável gera fraude interna e reprova em auditoria BACEN.

## Decisão
Ledger no padrão append-only. Nunca fazer UPDATE, só INSERT. Saldo é sempre calculado. Criar tabela `ledger_entries` com `entry_id`, `account_id`, `amount`, `type`, `previous_hash`. Hash encadeado para garantir imutabilidade.

## Consequências
Trilha de auditoria 100% rastreável, aprovação fácil em auditoria, prevenção de fraude.
