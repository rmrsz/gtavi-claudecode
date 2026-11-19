---
name: cortes-br
description: Rotina do canal de cortes de GTA 6 de um usuário que fala português. Modo atual é página em inglês com campanhas do Whop; o modo BR fica para quando houver campanhas brasileiras. Use quando o usuário falar em português, pedir "cortes", "canal de cortes", "campeonato de cortes", "rotina do dia" ou streamers BR.
---

# Cortes BR

Converse com o humano sempre em português do Brasil. Este é o formato escolhido para a página. Ele substitui as partes do `playbook` que dizem o contrário.

## Modo atual: página em inglês (escolhido em 19/11/2026)

Não havia campanha de GTA 6 com streamer brasileiro. O humano escolheu uma página em inglês com as campanhas pagas do Whop Content Rewards. Neste modo, as regras abaixo valem no lugar das partes em português do "Formato":

- **Fonte:** só streamers com campanha ativa no Whop Content Rewards (por exemplo KreekCraft, TOMO, Dylan Page em 19/11). Antes de cortar, confirme no `campaigns.md` que a campanha ainda tem orçamento.
- **País do público:** pule campanhas que exigem público dos EUA, Canadá, Reino Unido ou Austrália (várias da Reach). A conta é brasileira e não vai cumprir isso. Pule também campanhas patrocinadas por apostas.
- **Título e legenda:** em inglês, com a fórmula de reação do `hook-writer` (seção "Reaction titles"). Mostre ao humano o texto em inglês e uma tradução de uma linha em português, para ele saber o que está postando.
- **Plataformas:** TikTok, YouTube Shorts e Instagram Reels. Kwai não entra neste modo.
- **Lives:** `streams.py --query "gta 6"`, sem `--lang`.
- **Exigências da campanha:** overlay da live, hashtags e menções exigidas vão para o `campaigns.md` e são checadas pelo `check.py --name`.

## Modo BR (para quando houver campanhas brasileiras de GTA 6)

### Formato BR

- **Fonte:** lives brasileiras de GTA 6 na Twitch, Kick e YouTube. Só corte streamer que tem campanha ou campeonato de cortes ativo, ou que liberou cortes por escrito (painel da live, bio, Discord oficial). Anote a prova em `campaigns.md`.
- **Estilo:** reação. Rosto em cima, jogo embaixo, título grande na emenda (`--layout react --title-style box --captions word`). 15 a 35 segundos, terminando na reação, nunca depois.
- **Título:** em português, nome do streamer + verbo de reação + o que aconteceu, com 2 ou 3 palavras em CAIXA ALTA. Exemplos: "[Streamer] SURTOU com a POLÍCIA de Vice City", "Ele NÃO ACREDITOU no que achou no mapa". Use `hook-writer`.
- **Plataformas:** o mesmo arquivo vai para TikTok, YouTube Shorts, Instagram Reels e Kwai. Kwai conta views em campeonatos brasileiros como os da Cut.Pro; confira em cada campanha quais redes valem.
- **Volume:** semana 1 (lançamento), 4 a 6 cortes por dia. Semana 2, 3 a 4 cortes por dia, só nos streamers e tipos de momento que deram mais views.

## Rotina do dia (os dois modos)

1. `campaign-finder`: confira campanhas e campeonatos ativos (Whop Content Rewards, Clip, Cut.Pro e o Discord dos streamers). Atualize `campaigns.md`, um `## Nome` por campanha com `min_seconds`, `max_seconds`, `required_hashtags`, `required_mention`.
2. `python "${CLAUDE_PLUGIN_ROOT}/scripts/streams.py" --query "gta 6"` (no modo BR, com `--lang pt`): quem está ao vivo agora. Cruze com `campaigns.md`.
3. Live em andamento: `live.py https://twitch.tv/<login> --minutes 60`. Live que já acabou: `moments.py <url do VOD> --top 8`.
4. Corte os 3 melhores momentos com `cut.py ... --layout react --title "..."`.
5. `check.py <clip> --campaign campaigns.md --name "<campanha>" --caption "<legenda>"`. Só passa para o humano o que der PASS.
6. Mande ao humano uma única mensagem: arquivo, título, legenda com crédito (`🎥 @streamer`) e hashtags exigidas. Ele posta e envia o link na campanha.
7. Registre no `scoreboard.csv` com `learning-loop`. Leituras em 24 h e 72 h.

## Regras

- Nada de spoiler da história no título na semana de lançamento.
- Não invente fala nem opinião do streamer. O título descreve o que aconteceu.
- Palavrões ficam mascarados nas legendas por padrão. Use `--allow-profanity` só se a campanha permitir.
- Nada de música de fundo adicionada: dá remoção por direitos autorais.
- Nunca poste o mesmo corte duas vezes na mesma campanha, nem o mesmo link em dois envios.
- Nada de views compradas, bots ou contas falsas. Campanhas retêm o pagamento quando suspeitam.
- Ganhos: só registre o que a campanha mostrou como aprovado em `payouts.csv`. Nunca projete.
