---
name: geek-clipper
description: Geek - Clipper — transforma lives longas de streamers (YouTube, Twitch ou Kick) em cortes verticais prontos pra postar, com a câmera do streamer em cima, o jogo embaixo, título de gancho, legenda palavra por palavra e a legenda do post com hashtags. Use quando a pessoa disser "Geek - Clipper", "monta o Geek Clipper", "instala o clipper", "corta essa live", "faz cortes dessa live", "acha as maiores lives de GTA 6 de hoje e corta", ou colar um link de live pedindo cortes.
---

# Geek - Clipper

Versão do Geek nos Negócios baseada na skill gratuita [skill-clips](https://github.com/davixavier-coder/skill-clips) de davixavier-coder. Mantenha o crédito.

Você (a IA) faz só uma parte do trabalho: **ler os trechos quentes da live e escolher os momentos**. Todo o resto
(baixar, escutar, achar a câmera, montar o vídeo e a legenda) é feito pelos scripts, no computador da pessoa.

Fale com a pessoa em português simples, uma frase por vez, sem jargão. Ela não é programadora.

## Regras que evitam travar

1. **Sempre o atalho.** Depois de instalado, todo comando é:
   - Mac / Linux / Git Bash: `~/geek-clipper/clip COMANDO`
   - Windows PowerShell: `& "$HOME\geek-clipper\clip.cmd" COMANDO`
   Se o atalho não existir, instale (passo 0). Não procure Python, não crie ambiente, não instale pacote na mão.
2. **"AINDA TRABALHANDO" não é erro.** O trabalho pesado roda sozinho em segundo plano. Diga à pessoa em que
   ponto está (a porcentagem que apareceu) e rode **o mesmo comando de novo**. Nada recomeça do zero.
   Se o comando for cortado por tempo, também é só rodar de novo.
3. **Mensagem com "NÃO DÁ PRA BAIXAR" ou "AINDA ESTÁ AO VIVO"**: repita pra pessoa com essas palavras (é trava do
   site, não do clipper) e ofereça outra live (`buscar`). Nunca diga "o YouTube não deixa baixar".
4. Não tente baixar vídeo, ler site ou escrever script por conta própria. Tudo que precisa está nos comandos abaixo.
5. Se você não conseguir escrever o arquivo de momentos depois de 2 tentativas, use o modo automático:
   `escolher NOME --auto --manter N` e depois `fazer NOME`.

## 0. "Instala o Geek Clipper" / "atualiza o clipper"

Rode **um comando só** (baixa a skill, arruma o Python sozinho se faltar e instala tudo; uns 2 GB, pode levar minutos):

- Windows (PowerShell): `irm https://raw.githubusercontent.com/Geeknosnegocios/geek-clipper/main/instalar.ps1 | iex`
- Mac/Linux: `curl -fsSL https://raw.githubusercontent.com/Geeknosnegocios/geek-clipper/main/instalar.sh | bash`

Se a skill já está nesta pasta, também dá pra rodar direto: `python "$HOME\.claude\skills\geek-clipper\scripts\clip.py" instalar`.
Quando aparecer **TUDO PRONTO**, diga: "Pronto, o Geek Clipper tá instalado. É só me mandar o link de uma live (YouTube, Twitch ou Kick)."
Se der erro, rode o mesmo comando de novo uma vez. Se repetir, mostre à pessoa a última linha do erro.

## 1. "Acha as maiores lives de GTA 6 de hoje"

`buscar` (padrão: 7 lives, só as que já acabaram e dá pra baixar). Mostre a lista (título, canal, duração, views)
e pergunte quais cortar, ou corte a primeira se a pessoa já pediu pra cortar direto.

## 2. "Corta essa live" / "faz 5 cortes dessa live"

Número de cortes: o que a pessoa pediu (sem número = 3).

### 2.1 Preparar
`preparar LINK`. Baixa só o áudio, escuta a live inteira e separa os **trechos mais quentes** (gritos, chat
acelerado, reações) em `candidatos.txt`. Live longa demora: siga a regra 2 até aparecer **PRONTO PRA ESCOLHER**.
Se a live não for em português nem em inglês, passe `--idioma` (ex.: `--idioma es`).

### 2.2 Escolher os momentos (esta é a sua parte)
Leia **só** `~/geek-clipper/trabalho/NOME/candidatos.txt` (o caminho aparece no fim do `preparar`) e escreva
`~/geek-clipper/trabalho/NOME/momentos/candidatos.json`. Escreva uns 2 momentos a mais do que a pessoa pediu.

Cada linha é `[hh:mm:ss] fala`. As marcas ajudam:
- `[ALTO +12dB]`: o streamer ficou muito mais alto que o normal ali (grito, surto, susto).
- `[CHAT 3.1x]`: o chat andou 3 vezes mais rápido que o normal, alguma coisa aconteceu.
Marca sozinha não é momento. Confirme pela fala.

**O que vale um corte** (o que as pessoas compartilham):
- primeira reação a algo do jogo (ver o mapa, um personagem, um gráfico pela primeira vez)
- descoberta absurda ou detalhe que ninguém tinha visto
- surto, grito, raiva, susto
- falha engraçada, bug, morte ridícula
- opinião forte ou polêmica ("isso aqui tá pior que o GTA 5")
- história ou piada que fecha sozinha

**Seja rigoroso. Jogue fora:**
- o que precisa de contexto de antes pra fazer sentido
- o que começa com enrolação ("é…", "então…", "tipo assim", "voltando…", "uh, so yeah")
- leitura de doação, propaganda, conversa sobre a própria live, pausa, tela de carregamento
- fala picada ou que a transcrição não deixa entender

**Nota de 0 a 100.** 90+ = qualquer pessoa para de rolar o feed. 70–89 = bom. Abaixo de 60, nem escreva.
Nem todo trecho tem momento bom: não invente pra encher.

**Duração:** de 20 a 60 segundos. Comece 1–2 segundos antes da frase ou do grito que prende (nunca no meio de uma
palavra) e termine logo depois da reação/conclusão.

Formato de `momentos/candidatos.json`:

```json
[
  {
    "inicio": "01:12:03",
    "fim": "01:12:41",
    "nota": 88,
    "tipo": "primeira reação",
    "titulo": "ELE NÃO ACREDITOU NO TAMANHO DO MAPA",
    "porque": "grita ao ver o mapa inteiro; fecha sozinho sem precisar de contexto",
    "post": "O tamanho do mapa do GTA 6 deixou ele sem palavras 😳 #gta6 #gtavi #gta #cortes #streamer"
  }
]
```

- `titulo`: o gancho que vai escrito no vídeo. Curto (até ~45 letras), em MAIÚSCULAS, na língua da live, sem
  spoiler do final. Use o nome ou apelido do streamer quando ajudar ("CASIMIRO NÃO ACREDITOU…").
- `post`: legenda do post, 1–2 frases + 4 a 6 hashtags (sempre `#gta6`).

Se a pessoa quiser os melhores da live INTEIRA (mais lento), leia também os `blocos/bloco_NN.txt` e escreva
`momentos/bloco_NN.json` no mesmo formato.

### 2.3 Ficar com os melhores
`escolher NOME --manter N` (N = quantos cortes a pessoa pediu). Ele aceita o JSON mesmo com pequenos erros, junta
tudo, ordena pela nota e tira os que se sobrepõem. Conte: "Achei N momentos e fiquei com os melhores: …".

### 2.4 Montar os cortes
`fazer NOME`. Baixa só os segundos escolhidos, acha a câmera do streamer e monta o vídeo 1080×1920: câmera em cima,
jogo embaixo, título de gancho, legenda palavra por palavra. Sem câmera, usa o jogo inteiro com fundo desfocado.
Siga a regra 2 até aparecer **CORTES PRONTOS**.

Saída: `~/geek-clipper/cortes/NOME/corte_01.mp4` + `corte_01.txt` (legenda do post + crédito da live).
Diga onde estão e que é só postar. No Mac, abra a pasta pra pessoa: `open ~/geek-clipper/cortes/NOME`
(Windows: `explorer "$HOME\geek-clipper\cortes\NOME"`).

## Tudo automático (IA com dificuldade ou pessoa com pressa)
`automatico LINK --quantos N` faz preparar + escolher + fazer sozinho, pelos trechos mais quentes. Os títulos ficam
mais simples (a frase mais forte do trecho). Siga a regra 2.

## Várias lives
Rode `preparar` com um link por vez (cada um até ficar PRONTO PRA ESCOLHER), depois escolher e fazer pra cada uma.

## Problemas comuns
- `status` mostra em que passo cada live está e o que está rodando agora.
- Momento ruim: apague-o do `momentos/candidatos.json`, rode `escolher` de novo e `fazer` (só refaz o que mudou).
- Erro de download que não seja trava do site: `instalar` (atualiza o baixador) e o mesmo comando de novo.
- Demorando muito: no modo de graça o computador da pessoa faz tudo. Explique isso, é normal em PC mais simples.

## Respeito aos streamers
Corte só de streamers que permitem cortes (muitos têm canal oficial de cortes e liberam). O `.txt` de cada corte já
traz o crédito com o link da live: mantenha na postagem.
