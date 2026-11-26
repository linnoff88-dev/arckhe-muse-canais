ï»¿# muse-canais

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![muse-canais](guia/assets/banner.jpg)](https://linnoff88-dev.github.io/arckhe-/guia/)

**ê°Å¸â¡Â§ê°Å¸â¡Â· PortuguêÂªs êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

ê°Å¸ââ **Guia de uso (landing + passo a passo):** **https://linnoff88-dev.github.io/arckhe-/guia/**

## O que êÂ©

O muse-canais olha os seus canais do YouTube e mostra quais vêÂ­deos recentes estêÂ£o indo bem acima do normal de cada canal. Serve para quem publica muito e quer repetir o que funciona em vez de adivinhar. Ele lêÂª o feed pêÂºblico de cada canal (os 15 vêÂ­deos mais recentes, com as visualizaêÂ§êÂµes), calcula quantas visualizaêÂ§êÂµes por hora cada vêÂ­deo ganha e compara com o vêÂ­deo têÂ­pico do mesmo canal. Os que passam de 2 vezes viram "vencedores" e, se vocêÂª pedir, viram 8 ideias novas de vêÂ­deo. Precisa sêÂ³ de Python 3; as ideias usam o Claude Code pela assinatura.

## Usar

```bash
git clone https://github.com/linnoff88-dev/muse-canais ~/projetos/muse-canais
cd ~/projetos/muse-canais
python3 muse.py --atualiza-canais --dry   # descobre os canais e sêÂ³ mostra
python3 muse.py --ideias                  # grava relatêÂ³rio + 8 ideias
```

RelatêÂ³rio: `~/projetos/output/muse-canais/<data>/muse.md` (e `muse.json`).

| OpêÂ§êÂ£o | Faz |
|---|---|
| `--ideias` | 8 ideias novas a partir dos vencedores (e das pautas do radar-pautas do dia, se houver) |
| `--n 12` | quantas ideias |
| `--limiar 3` | quantas vezes acima do normal do canal para ser vencedor (padrêÂ£o 2) |
| `--canais lives1,lives11` | sêÂ³ esses canais |
| `--atualiza-canais` | relêÂª a lista de canais do yt-pubx (sêÂ³ nome e endereêÂ§o) e descobre o ID pêÂºblico de cada um |
| `--dry` | sêÂ³ imprime |

**Canais:** `canais.json` (chave, nome, endereêÂ§o, ID pêÂºblico). No ARCKHÉ ele nasce do yt-pubx; fora dele, escreva êÂ  mêÂ£o:
`{"meucanal": {"nome": "Meu canal", "url": "https://www.youtube.com/@meucanal", "id": "UCê¢â¬Â¦"}}` (o ID sai com `python3 -c "import muse; print(muse.resolver_id('https://www.youtube.com/@meucanal'))"`, rodado na pasta do projeto; `--atualiza-canais` sêÂ³ serve para quem usa o yt-pubx).

## Como a conta êÂ© feita

- **Velocidade** = visualizaêÂ§êÂµes êÂ· horas desde a publicaêÂ§êÂ£o. A partir da 2êÂª rodada, o histêÂ³rico (`estado/historico.jsonl`) tambêÂ©m guarda o ganho entre rodadas.
- **Normal do canal** = mediana da velocidade dos vêÂ­deos com mais de 6 h, com piso de 0,5 por hora (canal quase parado nêÂ£o infla o resultado).
- **Vencedor** = 2êâ o normal ou mais, entre 6 h e 30 dias de idade, com 50+ visualizaêÂ§êÂµes.

## O processo, do inêÂ­cio ao fim

ê°Å¸Â§Â  sessêÂ£o com o Nei êÂ· ê¢Å¡â¢ê¯Â¸Â script/timer êÂ· ê°Å¸Â¤â `claude -p` (assinatura)

| # | Passo | Comando / o que acontece | Quem |
|---|---|---|---|
| 1 | Canais | `muse.py --atualiza-canais` lêÂª nome e endereêÂ§o do yt-pubx e descobre o ID pêÂºblico | ê¢Å¡â¢ê¯Â¸Â |
| 2 | Leitura | feed RSS pêÂºblico de cada canal (15 vêÂ­deos, visualizaêÂ§êÂµes) | ê¢Å¡â¢ê¯Â¸Â |
| 3 | Conta | velocidade, normal do canal, vencedores; histêÂ³rico em `estado/` | ê¢Å¡â¢ê¯Â¸Â |
| 4 | Ideias | `--ideias`: 8 vêÂ­deos novos no padrêÂ£o dos vencedores (+ pautas do radar) | ê°Å¸Â¤â |
| 5 | Escolha | o Nei escolhe quais ideias viram vêÂ­deo | ê°Å¸Â§Â  |
| 6 | DiêÂ¡rio (opcional) | `systemd/muse-canais.timer` êÂ s 08:00 ê¢â¬â **vem desligado** | ê¢Å¡â¢ê¯Â¸Â |

## Limites

- O feed pêÂºblico sêÂ³ traz os 15 vêÂ­deos mais recentes de cada canal e nêÂ£o traz tempo assistido nem retenêÂ§êÂ£o.
- VisualizaêÂ§êÂµes do feed atrasam algumas horas em relaêÂ§êÂ£o ao YouTube Studio.
- As ideias veem sêÂ³ têÂ­tulo e nêÂºmeros: o conteêÂºdo de cada vêÂ­deo novo precisa de fonte prêÂ³pria.

LicenêÂ§a: MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>