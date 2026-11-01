# Lousa de Matemática Financeira (protótipo)

Um arquivo só (`lousa-matematica-financeira.html`), funciona offline. Você escolhe a incógnita, digita os dados e a lousa monta, em giz:

1. **Fluxo de caixa**: setas para cima (entradas, +) e para baixo (saídas, −, a tecla CHS), com o rótulo de cada valor. Prazos longos (ex.: 360) mostram os primeiros e os últimos períodos e indicam os omitidos no rodapé.
2. **Teclas da HP12C**: em teclas desenhadas, na ordem de digitação: `f CLEAR FIN`, `g BEG/END`, `f 2` (casas decimais) e um grupo por dado.
3. **Fórmula**: primeiro a fórmula com símbolos, depois a mesma fórmula com os valores colocados.
4. **Resultado**: no formato do visor da HP (ponto no milhar, vírgula decimal).

Todos os cálculos usam o motor da HP12C virtual (`/workspace/hp12c/engine.js`). O motor é embutido sem nenhuma alteração; o build só cola o arquivo dentro do HTML. A fórmula exibida é só a explicação. O número do resultado sai das teclas executadas no motor.

## Como usar

- **Calcular**: escolha n, i, PV, PMT ou FV. O campo da incógnita fica travado.
- **Sinais**: um valor negativo é uma saída de caixa e vira `CHS` nas teclas. Se PV e FV tiverem o mesmo sinal sem PMT, a HP dá **Error 5**, e a lousa mostra o aviso.
- **Campo vazio = 0**: o campo não entra nas teclas, porque o `f CLEAR FIN` já zerou o registrador. Digite `0` se quiser mostrar a tecla na lousa (ex.: `0 FV`, como na sequência original).
- **Pagamentos**: END (postecipado) ou BEG (antecipado). Muda o diagrama, as teclas (`g END` / `g BEG`) e a fórmula do PMT.
- **Casas decimais**: mudam o `f 2`/`f 4` e o arredondamento do visor.
- **n fracionário**: a HP12C arredonda n **para cima** (ex.: 9,5786 → 10,00). A lousa mostra o valor exato na fórmula e o valor da HP no resultado, com uma nota.
- **i** não tem fórmula fechada: a HP acha a taxa por iteração, e a lousa diz isso.

### Taxas equivalentes (juros compostos)

Na aba **Taxas equivalentes**, você converte do período curto para o longo (ex.: ao mês → ao ano) ou ao contrário. A conta usa as teclas de TVM da HP:

- curto → longo: `100 CHS PV`, a taxa no `i`, o número de períodos curtos no `n`, `FV`, `100 −`
- longo → curto: `100 CHS PV`, `100 + taxa` no `FV`, os períodos no `n`, `i`

A lousa mostra também a fórmula (1 + i)^k − 1.

## Exemplos prontos (casos verificados)

Ficam em `exemplos-lousa.json`, que vai embutido no HTML. Os valores esperados dos casos (a) a (e) são os casos de teste da HP12C virtual (`/workspace/hp12c/test.js`):

| Caso | Teclas | Visor |
|---|---|---|
| (a) FV | `1000 CHS PV 12 n 1 i FV` | 1.126,83 |
| (b) PMT END | `g END 100000 PV 1 i 360 n 0 FV PMT` | −1.028,61 |
| (c) i | `1000 CHS PV 1500 FV 24 n 0 PMT i` | 1,70 |
| (d) PMT BEG | `g BEG 100000 PV 1 i 360 n 0 FV PMT` | −1.018,43 |
| (e) FV | `5000 CHS PV 200 CHS PMT 24 n 1 i FV` | 11.743,37 |

Os extras (n = 12,00; PV = −1.000,00; 1% a.m. → 12,68% a.a.; 12% a.a. → 0,9489% a.m.) foram calculados pelo mesmo motor e conferidos com a fórmula nos testes.

## Aula passo a passo

- **Apresentar passo a passo (tela cheia)**: a lousa começa vazia.
- `→`, `Espaço` ou um clique na lousa avançam uma etapa. A ordem é: diagrama, cada grupo de teclas, fórmula, fórmula com valores e resultado.
- `←` volta uma etapa. `Home` recomeça, `End` mostra tudo e `Esc` sai.
- Fora da tela cheia, os botões ◀ ▶, **Mostrar tudo** e **Recomeçar**, abaixo da lousa, fazem o mesmo.

## Exportar PNG

- **PNG 1920×1080** (16:9, projetor/YouTube) e **PNG 1080×1920** (9:16, Reels/Stories). O PNG sai sempre com a lousa completa.
- Nome do arquivo: `lousa-calcular-fv-1920x1080.png`, `lousa-taxa-equivalente-1080x1920.png` etc.
- Exemplos gerados pelos testes estão em `exemplos-exportados/`.

## Arquivos

- `lousa-matematica-financeira.html`: o protótipo (gerado; não edite à mão).
- `src/index.html`: o código-fonte. Gere de novo com `python3 build.py lousa-matematica-financeira` na pasta `prototypes/`.
- `exemplos-lousa.json`: os exemplos prontos.
- Testes: `node _tests/test-lousa.js`.
- Capturas: `screenshot.png`, `screenshot-reel.png`, `screenshot-taxas.png` e `screenshot-revelacao*.png` (modo passo a passo).

## Limites do protótipo

- Só TVM (n, i, PV, PMT, FV) e taxa equivalente composta. Não cobre NPV/IRR (fluxos irregulares), amortização, datas nem juros simples.
- Os dados são digitados no formulário, não na lousa.
- As fontes (Inter/Montserrat) estão embutidas no arquivo. Não depende de internet.

> **Nesta versão publicada, o app é o `index.html`** (cópia de `lousa-matematica-financeira.html`).
