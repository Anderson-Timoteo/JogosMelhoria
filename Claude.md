# Jogo 5S · Exercício dos Números — Especificação para desenvolvimento

> Documento de referência para construir, no Claude Code, uma versão **web e jogável** do exercício dos números do 5S (slides 48 a 61 do `Treinamento_Lean_5S_PEL_Master.html`), publicada no **GitHub Pages**. Assim, os participantes só abrem um link no celular, sem baixar nada.

---

## 1. Objetivo

Mostrar na prática, com o celular de cada participante, como cada senso do 5S melhora o desempenho num mesmo trabalho:

> **"O mesmo trabalho, as mesmas pessoas: só o ambiente mudou."**

A pessoa encontra os números de **1 a 49 em ordem**, tocando neles. O quadro fica mais organizado a cada rodada (sem 5S → 1º S → … → 5º S), e no final ela compara as próprias pontuações.

## 2. Contexto

- **Programa:** PEL · Programa de Excelência Logística (Assaí · Supply / CDs). Nunca usar "PELM". Não citar Leroy Merlin.
- **Uso:** treinamento presencial, com o facilitador projetando o material master e os participantes jogando no próprio celular pelo link (QR code no slide).
- **Público:** time operacional e administrativo de CD, com pouca familiaridade com tecnologia. A interface precisa ser simples, com letras grandes e botões grandes.

## 3. Requisitos técnicos

| Item | Definição |
|---|---|
| Entrega | **Um único `index.html`**, com CSS e JS embutidos, sem build e sem dependências obrigatórias |
| Hospedagem | GitHub Pages (repositório público, branch `main`, raiz) |
| Dispositivo principal | **Celular em modo retrato** (360–430 px de largura); também precisa funcionar em desktop e tablet |
| Offline | Funcionar depois de carregado; fontes externas são opcionais, com fallback `system-ui` |
| Dados | Nada vai para servidor. As pontuações ficam em `localStorage`, envolvido em `try/catch` |
| Idioma | Português (pt-BR) |
| Acessibilidade | Alvos de toque ≥ 44 px, contraste AA e `prefers-reduced-motion` respeitado |

## 4. Identidade visual (padrão "Evoluir")

```css
--pd:#2B0F4C;  /* roxo escuro (fundos, títulos) */
--p: #4A1D7A;  /* roxo */
--pm:#8E6BBF;  /* lilás */
--pl:#EDE6F7;  /* lavanda (fundos claros) */
--y: #FFC20E;  /* amarelo (destaques, botões) */
--yl:#FFF3C4;  /* amarelo claro */
--r: #D64545;  /* vermelho (erro, alerta) */
--gr:#2E9E6B;  /* verde (acerto) */
--ink:#1F1530; /* texto */
```

- **Fonte:** Poppins (Google Fonts), com fallback `system-ui, "Segoe UI", Arial`.
- **Cores dos sensos:** Seiri `#FFC20E` · Seiton `#EE7B22` · Seiso `#D64545` · Seiketsu `#8E6BBF` · Shitsuke `#4A1D7A`.
- **Faixa da marca** no rodapé (7 cores): `#F6B513 #EE7B22 #C8202F #4A86C5 #5A9A3E #2C4E8A #7B3F98`.
- **Estilo:** cards arredondados (18–28 px), sombras suaves, animações curtas de entrada e o roxo vivo nas telas de abertura.

## 5. Fluxo do jogo

```
[Tela inicial] → [Como jogar] → Rodada 0 (Sem 5S) → [Pontuação]
   → Senso 1 (explicação) → contagem 10 s → Rodada 1 → [Pontuação]
   → Senso 2 → contagem → Rodada 2 → [Pontuação]
   → … até o Senso 5 / Rodada 5
   → [Resultado final: gráfico comparando as 6 rodadas]
```

### 5.1 Tela inicial
- Título "Exercício dos Números · 5S", subtítulo "PEL · Programa de Excelência Logística".
- Campo opcional "Seu nome" (só aparece no resultado e na captura de tela).
- Botão grande **"Começar"**.
- Link discreto "Como jogar".

### 5.2 Como jogar
- "Toque nos números de **1 até 49, em ordem**."
- "Você tem **1 minuto** por rodada."
- "Sua pontuação é o último número certo que você encontrou."
- "A cada rodada o quadro fica mais organizado: veja quanto você melhora."

### 5.3 Tela de explicação do senso (antes das rodadas 1 a 5)
- Card com: número do senso, nome em japonês (kanji + romaji), nome em português, ícone, cor do senso e as dicas de aplicação (seção 8).
- Botão **"Estou pronto"**, que inicia uma **contagem de 10 s** em tela cheia (10 → 1, número grande pulsando) e abre a rodada.
- Opcional: a barra de 20 s de explicação do material master, só no modo apresentador (seção 10).

### 5.4 Rodada (o jogo)
- **Cronômetro de 60 s** no topo (anel + `0:59`); nos últimos 10 s fica vermelho e pulsa.
- Indicador "Próximo: **12**" sempre visível.
- **Toque correto:** o número fica marcado (círculo verde translúcido + leve escala), o próximo é atualizado e há vibração curta (`navigator.vibrate(15)`, se existir).
- **Toque errado:** o número tremula em vermelho por 300 ms. Não há penalidade, só feedback.
- Números acima de 49 (rodada 0) são "itens desnecessários": tocar neles conta como erro.
- **Fim por tempo:** "TEMPO!" + pontuação. **Fim antecipado:** se chegar ao 49, "Completou em X s!".
- Registrar por rodada: `pontos` (último correto), `tempo` (s), `erros` (toques errados).

### 5.5 Tela de pontuação da rodada
- Pontuação grande, comparação com a rodada anterior (▲ +7 / ▼ −2) e mini gráfico das rodadas já feitas.
- **Comparar quadros:** um botão alterna entre o quadro da rodada **anterior** e o **atual**, para ver o que mudou. Pedido do usuário; pode ser um slider "antes | depois" ou dois botões com transição suave.
- Botão "Próximo senso".

### 5.6 Resultado final
- Gráfico de barras animado com as 6 rodadas (Sem 5S, 1º S … 5º S), com as cores dos sensos.
- Texto: "Você saiu de **12** para **49** pontos (+308%)."
- Frase final: "O mesmo trabalho, as mesmas pessoas: só o ambiente mudou."
- Botões: **"Jogar de novo"** (zera o progresso), **"Salvar imagem"** (print do resultado via `canvas`, opcional) e **"Comparar quadros"** (galeria antes/depois das 6 rodadas).

## 6. Os quadros (6 estágios)

Área lógica de **1440 × 640**, escalada para caber na tela. **No celular em retrato, girar o quadro para 640 × 1440** (trocar x↔y) ou usar um layout vertical equivalente. O importante é que os números fiquem grandes o suficiente para tocar (≥ 40 px de área de toque).

| Rodada | Senso | O que muda no quadro |
|---|---|---|
| 0 | Sem 5S | 70 números (1 a 70) espalhados, tamanhos 18–41 px, rotação −60° a +60°, 5 cores, 16 manchas rosa translúcidas ("sujeira") |
| 1 | Seiri · Selecionar | Mesmas posições, mas **só 1–49** (os números de 50 a 70 saem); manchas continuam |
| 2 | Seiton · Organizar | 10 zonas tracejadas (5 × 2) com rótulo "1–5", "6–10"…; cada zona tem os seus 5 números, ainda com tamanho, cor e rotação variados; manchas continuam |
| 3 | Seiso · Limpar | Mesmas zonas, **sem manchas**, todos os números com 30 px, cor `#1F1530` e sem rotação |
| 4 | Seiketsu · Padronizar | Tabela 10 × 5 com 1–49 em ordem (última célula vazia) |
| 5 | Shitsuke · Manter | Mesma tabela com linhas zebradas, múltiplos de 10 destacados em roxo e "FIM" na última célula |

### 6.1 Geração determinística (igual ao material master)

```js
let sd = 7;
const rnd = () => { sd = (sd * 9301 + 49297) % 233280; return sd / 233280; };
const GW = 1440, GH = 640;

// posições sem sobreposição
const basePos = [];
for (let i = 0; i < 70; i++) {
  let p, t = 0;
  do { p = { x: 20 + rnd() * (GW - 100), y: 15 + rnd() * (GH - 80) }; t++; }
  while (t < 300 && basePos.some(q => Math.abs(q.x - p.x) < 82 && Math.abs(q.y - p.y) < 58));
  basePos.push(p);
}
const gSz  = basePos.map(() => 18 + Math.floor(rnd() * 24));
const gRot = basePos.map(() => Math.floor(rnd() * 120) - 60);
const gCol = basePos.map(() => ["#1F1530","#4A1D7A","#D64545","#8E6BBF","#5E5872"][Math.floor(rnd() * 5)]);
const gNum = Array.from({ length: 70 }, (_, i) => i + 1).sort(() => rnd() - 0.5);
const blobs = Array.from({ length: 16 }, () => ({
  x: rnd() * (GW - 260), y: rnd() * (GH - 160), w: 150 + rnd() * 230, h: 80 + rnd() * 130 }));

// Rodadas 2 e 3 (zonas): zona z = 0..9 → zx = (z%5)*GW/5, zy = floor(z/5)*GH/2
// slots dentro da zona: [[30,50],[150,70],[80,150],[170,200],[40,230]]
// número n = z*5+k+1 (até 49) → slot = slots[(n*3)%5]; estilo: gSz/gCol/gRot[n%70] (rodada 2) ou fixo (rodada 3)
// Rodadas 4 e 5 (tabela): célula (n-1)%10 × floor((n-1)/10), 144 × 128 cada
```

- Usar a mesma semente para que todos vejam o **mesmo quadro**, o que deixa a comparação da turma justa.
- Opcional: botão "embaralhar" (nova semente) só para treino individual.
- Os números precisam ser elementos tocáveis (`<button>` ou elemento com `role="button"`) com área de toque maior que o glifo (padding invisível), sobretudo os pequenos e girados.
- Manchas com `pointer-events: none`.

## 7. Textos dos sensos

| Senso | Kanji | Romaji | Português | Letra |
|---|---|---|---|---|
| 1º | 整理 | SEIRI | Senso de Utilização (Selecionar) | S |
| 2º | 整頓 | SEITON | Senso de Organização (Ordenar) | O |
| 3º | 清掃 | SEISO | Senso de Limpeza (Limpar) | L |
| 4º | 清潔 | SEIKETSU | Senso de Padronização (Padronizar) | P |
| 5º | 躾 | SHITSUKE | Senso de Autodisciplina (Manter) | M |

Fonte dos kanji: `"Yu Gothic","Meiryo","Hiragino Sans","Noto Sans JP",sans-serif`.

## 8. Dicas de aplicação (tela de explicação)

**1º Seiri**
- Separar o necessário do desnecessário e descartar o que não será usado
- Levantar recursos (empilhadeiras, paleteiras, coletores, materiais) e identificar o que agrega valor
- Não deixar materiais sobre mesas, estantes, corredores e áreas comuns
- Ao sair, não deixar nenhum material sobre o posto de trabalho

**2º Seiton**
- Definir critérios de alocação para que cada item tenha o seu lugar
- Definir um layout adequado para os recursos nos locais estabelecidos
- Padronizar nomes e locais e identificar tudo visualmente (endereços, demarcações)

**3º Seiso**
- Rotina diária de limpeza: cada um cuida do seu espaço
- Usar listas de verificação para garantir cada área limpa
- Evitar comer no local de trabalho
- Identificar e eliminar as fontes de sujeira
- Dar destino correto ao lixo: coleta seletiva, filme e paletes

**4º Seiketsu**
- Criar instruções visuais para manter o local conforme o padrão
- Promover auditorias regulares de conformidade
- Identificar e eliminar fontes de risco e insalubridade
- Coletar feedback das equipes e revisar o padrão quando necessário

**5º Shitsuke**
- Líderes são modelo de disciplina e compromisso com o 5S
- Reconhecer o esforço e incentivar a criatividade
- Educar-se e educar continuamente, com paciência e persistência
- Incentivar o compromisso individual com as boas práticas

## 9. Estado e persistência

```js
// chave: "pel5s-progresso"
{
  nome: "Ana",
  rodadaAtual: 3,            // 0..5, ou "fim"
  rodadas: [
    { pontos: 12, tempo: 60, erros: 5 },
    { pontos: 21, tempo: 60, erros: 2 },
    ...
  ],
  semente: 7
}
```

- Se a pessoa recarregar a página, ela continua de onde parou (com a opção "Recomeçar").
- "Jogar de novo" limpa a chave.

## 10. Modo apresentador (opcional, via `?modo=apresentador`)

Para projetar no telão sem depender do material master:
- Barra de **20 s de explicação** nas telas de senso, com pausar e pular, seguida automaticamente da contagem de 10 s.
- Quadro em tela cheia (paisagem), sem registrar toques.
- Teclas: `→` avança, `←` volta, `F` tela cheia.

## 11. Estrutura sugerida do repositório

```
pel-5s-numeros/
├── index.html      # o jogo completo (HTML + CSS + JS embutidos)
├── og-image.png    # prévia para WhatsApp/Teams (1200×630), opcional
└── README.md       # como jogar + link do GitHub Pages
```

- `<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">`
- Tags Open Graph (título, descrição e imagem) para a prévia ficar boa ao compartilhar o link.
- No slide 48 do material master, trocar o botão "Começar" por um **QR code do link** do GitHub Pages.

## 12. Critérios de aceite

- [ ] Abre pelo link do GitHub Pages no Android (Chrome) e no iPhone (Safari) sem baixar nada.
- [ ] Em celular retrato, todos os números são tocáveis e não ficam cortados.
- [ ] Toque na ordem certa marca o número; toque errado dá feedback e não avança.
- [ ] Cronômetro de 60 s por rodada, com "TEMPO!" no fim; completar o 49 encerra antes.
- [ ] Contagem de 10 s antes de cada rodada de senso.
- [ ] 6 quadros exatamente como na seção 6, com a mesma semente para todos.
- [ ] Botão para alternar entre o quadro anterior e o atual.
- [ ] Resultado final com gráfico das 6 rodadas e a porcentagem de melhoria.
- [ ] Progresso mantido ao recarregar; "Jogar de novo" zera tudo.
- [ ] Identidade visual roxo + amarelo, PEL e Assaí, sem "PELM" e sem Leroy Merlin.
- [ ] Um único `index.html`, sem build, funcionando também ao abrir o arquivo localmente.

## 13. Fora do escopo (por enquanto)

- Ranking online da turma ou envio de pontuações para um servidor.
- Login.
- Integração com o material master (além do QR code).
