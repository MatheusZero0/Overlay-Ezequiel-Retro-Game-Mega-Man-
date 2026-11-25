# Retro.X | Overlay Retrô "Mega Man" para OBS

Overlay animado em **1920×1080** para transmissões de **jogos retrô** (Mega Drive, Super Nintendo e similares) no **OBS Studio**. Tema futurista em azul, inspirado no universo do Mega Man, feito em HTML, CSS e JavaScript. Funciona como **Fonte Navegador**, sem plugins.

## Destaques

- **5 cenas prontas:** Intro, Live, Game, Final e BRB (tela de "Pause").
- **Híbrido:** Intro e Final usam artes ilustradas, com animação por cima. Live, Game e BRB são desenhadas 100% em código.
- **Visual retrô 16 bits:** fonte pixel, grade de pixels, linhas de varredura de TV antiga, faixas azuis em movimento e partículas de pixel.
- **Barra de energia** no estilo Mega Man para a meta de inscritos.
- **Alertas temáticos com som 8 bits:** 1UP (inscrito), E-TANK (membro), PARAFUSO (Super Chat) e ALERTA DE CHEFE (raid).
- **Contagem regressiva** na Intro, com "READY!" ao terminar.
- **Modo de teste** por teclado, sem precisar esperar um evento real.
- **Jogo grande:** a janela do jogo ocupa 1468×826 na cena Live.

---

## 1. Conteúdo do pacote

```
megaman/
├── overlay-megaman.html    (o overlay, com as 5 cenas)
├── intro.jpg               (arte "Começando em breve")
├── final.jpg               (arte "Obrigado por assistir")
├── logo-retrox.png         (logo Retro.X com fundo transparente)
└── README.md
```

> Os 4 arquivos de overlay precisam ficar **na mesma pasta**. Se faltar uma imagem, o espaço dela aparece vazio.

## 2. Requisitos

- **OBS Studio** com suporte a Fonte Navegador (versões atuais já incluem).
- Canvas/saída do OBS em **1920×1080**.
- **Internet** na primeira abertura, para carregar as fontes do Google Fonts (*Press Start 2P* e *Chakra Petch*). Sem internet, entram fontes reserva.
- Opcional: conta no **Restream** (chat) e no **StreamElements** (alertas reais).

---

## 3. Instalação

1. Baixe o projeto: no GitHub, clique em **Code, Download ZIP** e extraia.
2. Mova a pasta para um lugar **fixo**, por exemplo `C:\megaman\`. Se mover depois, será preciso trocar as URLs no OBS.
3. Anote o caminho completo do arquivo `overlay-megaman.html`.

---

## 4. Montando as cenas no OBS

Crie **5 cenas** (os nomes são livres): Intro, Live, Game, Final e BRB.

Em cada cena, adicione uma fonte **Navegador** (`+` em Fontes, depois **Navegador**):

- **Desmarque "Arquivo local".** Se ficar marcado, o `?scene=` é ignorado e todas as cenas mostram a Intro.
- **Largura:** `1920`
- **Altura:** `1080`
- **URL:** conforme a tabela.

| Cena | URL (Windows) |
|---|---|
| Intro | `file:///C:/megaman/overlay-megaman.html?scene=intro` |
| Live | `file:///C:/megaman/overlay-megaman.html?scene=live` |
| Game | `file:///C:/megaman/overlay-megaman.html?scene=game` |
| Final | `file:///C:/megaman/overlay-megaman.html?scene=end` |
| BRB | `file:///C:/megaman/overlay-megaman.html?scene=brb` |

**Dicas sobre o caminho**
- Use barras normais `/`, nunca `\`.
- Espaço no nome da pasta vira `%20` (ex.: `Minha%20Pasta`).
- macOS: `file:///Users/seunome/megaman/overlay-megaman.html?scene=intro`
- Linux: `file:///home/seunome/megaman/overlay-megaman.html?scene=intro`

### Configurações recomendadas em cada fonte Navegador

- **Desligar fonte quando não estiver visível:** marcado
- **Atualizar navegador quando a cena ficar ativa:** marcado (a contagem da Intro reinicia a cada entrada)
- **FPS personalizado:** 30
- **Controlar áudio via OBS:** marcado (sem isso o som dos alertas não vai para a live)

---

## 5. Posições de cada cena

Nas cenas **Live** e **Game**, o jogo e a câmera ficam **por baixo** do overlay, que tem "janelas" transparentes para eles aparecerem. Use estes valores em *Transformar, Editar transformação*.

### Cena Live

| Fonte | Posição X, Y | Tamanho |
|---|---|---|
| Captura do jogo | 26, 26 | 1468 × 826 |
| Câmera | 1526, 26 | 368 × 277 |
| Chat do Restream | 1526, 381 | 368 × 673 |

**Ordem das fontes (de cima para baixo):**
1. Chat do Restream (**por cima de tudo**)
2. Overlay (Fonte Navegador `?scene=live`)
3. Câmera
4. Captura do jogo

### Cena Game (jogo em tela cheia)

| Fonte | Posição X, Y | Tamanho |
|---|---|---|
| Captura do jogo | 0, 0 | 1920 × 1080 |
| Câmera | 28, 818 | 318 × 234 |

**Ordem:** overlay no topo, depois a câmera, depois o jogo.

### Intro, Final e BRB

Só o overlay, sem câmera nem jogo. Dá para adicionar música de fundo como outra fonte.

---

## 6. Dicas para jogos retrô (Mega Drive, Super Nintendo)

Jogos antigos costumam ter proporção **4:3** (ou parecida), enquanto a janela do jogo é **16:9**. Opções:

- **Manter a proporção (recomendado):** ajuste a captura para a **altura de 826** e centralize na janela. Sobram faixas escuras nas laterais, que mostram o fundo do overlay. Em 4:3, a largura fica em torno de 1101 pixels.
- **Esticar para preencher:** distorce a imagem, então só vale se ele gostar do resultado.
- **Pixels nítidos:** clique com o botão direito na captura, vá em **Transformar** e procure a opção de **filtro de escala** (*Scaling Filter*), escolhendo **Ponto**. Os pixels ficam firmes em vez de borrados. O nome e o local exato podem variar conforme a versão do OBS.
- **Escala inteira:** se o emulador permitir escala em números inteiros, a imagem fica mais limpa.

---

## 7. Configurando textos, redes e meta

Abra o `overlay-megaman.html` em um editor de texto (o Bloco de Notas serve) e procure o bloco `CFG`:

```js
const CFG={
  brbMsg:"Pausa rápida. Já já a gente volta!",
  lastFollower:"—",
  socials:["YouTube: /Retro.X","Twitch: /Retro.X","Instagram: @Retro.X"],
  countdownMin:5,
  goal:{label:"META",current:0,target:100},
  seJWT:"",
  volume:0.12
};
```

| Campo | Para que serve |
|---|---|
| `brbMsg` | Mensagem da tela de pausa |
| `lastFollower` | Texto inicial de "último 1UP" na Live |
| `socials` | Redes mostradas na Live e no Final (3 ou 4 cabem bem) |
| `countdownMin` | Minutos da contagem da Intro |
| `goal` | Nome, valor atual e valor alvo da meta |
| `seJWT` | Token do StreamElements para alertas reais (seção 9) |
| `volume` | Volume dos sons dos alertas, de 0 a 1 |

Salve o arquivo e, no OBS, clique com o botão direito na fonte e escolha **Atualizar cache do navegador atual**.

> A contagem da Intro e a meta voltam ao valor do `CFG` sempre que a página recarrega.

**Sobre as redes da Intro:** na imagem `intro.jpg`, os ícones e nomes das redes fazem parte da arte. Alterar o `CFG` **não muda** a Intro. Para mudar, é preciso substituir a imagem.

---

## 8. Chat ao vivo (Restream)

O quadro de chat da Live é um espaço reservado: o chat de verdade vem do Restream.

1. No Restream, abra a área de **Chat** e copie o link de **Embed para OBS**.
2. No OBS, na cena Live, adicione uma fonte **Navegador** com esse link.
3. Tamanho `368 × 673`, posição `1526, 381`.
4. Deixe essa fonte **acima** do overlay.

O visual do chat depende das opções do Restream na sua conta.

---

## 9. Alertas

### Tipos de alerta

| Evento | Alerta no overlay | Ícone |
|---|---|---|
| Novo inscrito | **1UP! NOVO INSCRITO** (soma na meta) | Capacete pixel |
| Novo membro | **E-TANK! NOVO MEMBRO** | Tanque de energia |
| Super Chat | **PARAFUSO! SUPER CHAT** | Parafuso |
| Raid | **ALERTA DE CHEFE! RAID** | Invasor |

Alertas seguidos entram em **fila** e aparecem um de cada vez.

### Testar sem esperar a live

Abra o HTML no navegador (clique duplo) e use as teclas. No OBS, clique com o botão direito na fonte, escolha **Interagir** e use as mesmas teclas.

| Tecla | Efeito |
|---|---|
| `1` a `5` | Troca de cena (Intro, Live, Game, Final, BRB) |
| `Q` | Alerta de novo inscrito |
| `W` | Alerta de novo membro |
| `E` | Alerta de Super Chat |
| `R` | Alerta de raid |
| `C` | Liga/desliga chat falso (só para teste visual) |
| `M` | Soma 1 na meta |

### Alertas reais do YouTube (StreamElements)

1. Crie uma conta no **StreamElements** e conecte o canal do YouTube.
2. Copie o **JWT Token** (Account, Channels, Show secrets).
3. Cole em `seJWT:""` dentro do `CFG` e salve.
4. Use o botão de teste de alertas do StreamElements. Se o alerta aparecer no overlay, está funcionando.

**Atenção:**
- Trate o token como uma senha. **Nunca suba o arquivo com o token preenchido para o GitHub.**
- O mapeamento entre os eventos do StreamElements e os alertas do YouTube (inscrito, membro, Super Chat, raid) foi feito por suposição. Se algum alerta não aparecer, ajuste a função `fromSE` no código.
- Cada cena é uma página separada: um alerta real só aparece na cena que estiver ativa no momento.

---

## 10. Desempenho

Partículas, brilhos e efeitos de tela consomem placa de vídeo. Em caso de engasgo:

1. Confirme as opções da seção 4 (desligar fonte quando não visível e FPS 30).
2. Em **Configurações, Avançado**, deixe ligada a aceleração de hardware da Fonte Navegador.
3. Reduza as partículas: no código, procure `Array.from({length:60}` e troque `60` por um número menor.
4. Em emulação de consoles de 16 bits o jogo costuma ser leve, então normalmente há folga para o overlay.

---

## 11. Personalização

- **Cores:** no começo do CSS, as variáveis `--b1` (azul), `--c` (ciano), `--y` (amarelo) e `--r` (vermelho).
- **Trocar as artes:** substitua `intro.jpg` e `final.jpg` por imagens **1920×1080** com os mesmos nomes.
  - A contagem da Intro fica em um espaço livre à direita (`.cd` no CSS). Se a nova arte tiver algo ali, ajuste `left` e `top` dessa classe.
  - O painel de redes do Final fica no canto inferior direito (estilo inline da seção `end`). Ajuste se necessário.
- **Trocar o logo:** substitua `logo-retrox.png` por um PNG com fundo transparente.
- **Textos da BRB, metas e redes:** pelo `CFG` (seção 7).
- **Meta com mais ou menos blocos:** a barra tem 24 blocos. Para mudar, edite o número `24` nas funções `setGoal` e na criação da barra (`'<i></i>'.repeat(24)`).

---

## 12. Solução de problemas

| Problema | Causa provável | Solução |
|---|---|---|
| Todas as cenas mostram a Intro | "Arquivo local" está marcado | Desmarque e use a URL com `?scene=` |
| Imagens ou logo não aparecem | Arquivos fora da pasta do HTML, ou nomes diferentes | Deixe tudo na mesma pasta, com os nomes originais |
| Tela em branco | Caminho da URL errado | Confira barras `/`, `%20` nos espaços e o nome do arquivo |
| Fonte pixel não aparece | Sem internet na primeira carga | Conecte e atualize o cache da fonte |
| Sem som nos alertas | Áudio não controlado pelo OBS | Marque **Controlar áudio via OBS** |
| Jogo ou câmera "cobertos" | Ordem das fontes errada | Overlay acima do jogo e da câmera (o chat fica acima do overlay) |
| Janela do jogo desalinhada | Posição ou tamanho diferentes | Use os valores da seção 5 |
| Jogo 4:3 com laterais vazias | Proporção diferente da janela 16:9 | Veja a seção 6 |
| Contagem não reinicia | Opção de atualizar desmarcada | Marque **Atualizar navegador quando a cena ficar ativa** |

---

## 13. Avisos

- Os recursos foram escritos e revisados no código, mas **não foram testados em uma live real** durante a criação. Faça o roteiro de teste da seção 9 antes de entrar ao vivo.
- As artes da Intro e do Final foram geradas por IA pelo dono do canal.
- *Mega Man* é uma marca da **Capcom**. Este projeto é conteúdo de fã, sem vínculo oficial com a Capcom. Confira também as regras da plataforma de transmissão sobre uso de marcas e jogos de terceiros.
