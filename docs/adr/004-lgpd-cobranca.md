# ADR 004 - LGPD na Régua de Cobrança

**Status:** Aceita
**Data:** 2026-10-12

## Contexto
Ligações e WhatsApp de cobrança sem consentimento geram multa de até R$50MM pela ANPD.

## Decisão
Criar tabela de consentimentos. Antes de qualquer contato, checar `consent_whatsapp` e `consent_voz`. Gravar opt-in com timestamp, IP e texto aceito. Respeitar horário comercial (8h-20h seg-sex).

## Consequências
Mitiga risco de multa LGPD, facilita prova de consentimento em processo.
