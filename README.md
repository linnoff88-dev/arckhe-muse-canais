# muse-canais

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![muse-canais](guia/assets/banner.jpg)](https://inematds.github.io/muse-canais/guia/)

**ðŸ‡§ðŸ‡· PortuguÃªs Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

ðŸ“– **Guia de uso (landing + passo a passo):** **https://inematds.github.io/muse-canais/guia/**

## O que Ã©

O muse-canais olha os seus canais do YouTube e mostra quais vÃ­deos recentes estÃ£o indo bem acima do normal de cada canal. Serve para quem publica muito e quer repetir o que funciona em vez de adivinhar. Ele lÃª o feed pÃºblico de cada canal (os 15 vÃ­deos mais recentes, com as visualizaÃ§Ãµes), calcula quantas visualizaÃ§Ãµes por hora cada vÃ­deo ganha e compara com o vÃ­deo tÃ­pico do mesmo canal. Os que passam de 2 vezes viram "vencedores" e, se vocÃª pedir, viram 8 ideias novas de vÃ­deo. Precisa sÃ³ de Python 3; as ideias usam o Claude Code pela assinatura.

## Usar

```bash
git clone https://github.com/inematds/muse-canais ~/projetos/muse-canais
cd ~/projetos/muse-canais
python3 muse.py --atualiza-canais --dry   # descobre os canais e sÃ³ mostra
python3 muse.py --ideias                  # grava relatÃ³rio + 8 ideias
```

RelatÃ³rio: `~/projetos/output/muse-canais/<data>/muse.md` (e `muse.json`).

| OpÃ§Ã£o | Faz |
|---|---|
| `--ideias` | 8 ideias novas a partir dos vencedores (e das pautas do radar-pautas do dia, se houver) |
| `--n 12` | quantas ideias |
| `--limiar 3` | quantas vezes acima do normal do canal para ser vencedor (padrÃ£o 2) |
| `--canais lives1,lives11` | sÃ³ esses canais |
| `--atualiza-canais` | relÃª a lista de canais do yt-pubx (sÃ³ nome e endereÃ§o) e descobre o ID pÃºblico de cada um |
| `--dry` | sÃ³ imprime |

**Canais:** `canais.json` (chave, nome, endereÃ§o, ID pÃºblico). No INEMA ele nasce do yt-pubx; fora dele, escreva Ã  mÃ£o:
`{"meucanal": {"nome": "Meu canal", "url": "https://www.youtube.com/@meucanal", "id": "UCâ€¦"}}` (o ID sai com `python3 -c "import muse; print(muse.resolver_id('https://www.youtube.com/@meucanal'))"`, rodado na pasta do projeto; `--atualiza-canais` sÃ³ serve para quem usa o yt-pubx).

## Como a conta Ã© feita

- **Velocidade** = visualizaÃ§Ãµes Ã· horas desde a publicaÃ§Ã£o. A partir da 2Âª rodada, o histÃ³rico (`estado/historico.jsonl`) tambÃ©m guarda o ganho entre rodadas.
- **Normal do canal** = mediana da velocidade dos vÃ­deos com mais de 6 h, com piso de 0,5 por hora (canal quase parado nÃ£o infla o resultado).
- **Vencedor** = 2Ã— o normal ou mais, entre 6 h e 30 dias de idade, com 50+ visualizaÃ§Ãµes.

## O processo, do inÃ­cio ao fim

ðŸ§  sessÃ£o com o Nei Â· âš™ï¸ script/timer Â· ðŸ¤– `claude -p` (assinatura)

| # | Passo | Comando / o que acontece | Quem |
|---|---|---|---|
| 1 | Canais | `muse.py --atualiza-canais` lÃª nome e endereÃ§o do yt-pubx e descobre o ID pÃºblico | âš™ï¸ |
| 2 | Leitura | feed RSS pÃºblico de cada canal (15 vÃ­deos, visualizaÃ§Ãµes) | âš™ï¸ |
| 3 | Conta | velocidade, normal do canal, vencedores; histÃ³rico em `estado/` | âš™ï¸ |
| 4 | Ideias | `--ideias`: 8 vÃ­deos novos no padrÃ£o dos vencedores (+ pautas do radar) | ðŸ¤– |
| 5 | Escolha | o Nei escolhe quais ideias viram vÃ­deo | ðŸ§  |
| 6 | DiÃ¡rio (opcional) | `systemd/muse-canais.timer` Ã s 08:00 â€” **vem desligado** | âš™ï¸ |

## Limites

- O feed pÃºblico sÃ³ traz os 15 vÃ­deos mais recentes de cada canal e nÃ£o traz tempo assistido nem retenÃ§Ã£o.
- VisualizaÃ§Ãµes do feed atrasam algumas horas em relaÃ§Ã£o ao YouTube Studio.
- As ideias veem sÃ³ tÃ­tulo e nÃºmeros: o conteÃºdo de cada vÃ­deo novo precisa de fonte prÃ³pria.

LicenÃ§a: MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>