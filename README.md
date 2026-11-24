# passeio-de-drone

Skill do Claude Code que transforma as fotos de um imóvel vazio num passeio de drone de 30 segundos. Cada cômodo aparece vazio e vira mobiliado, e o drone atravessa a casa sem corte.

As imagens e o vídeo saem pelo conector MCP da Higgsfield: Nano Banana para limpar e mobiliar as fotos, Seedance 2.5 para o voo.

Vídeo que mostra o processo: https://youtu.be/ZPaqZcA2-O0

## A ideia

O prompt descreve, a foto trava.

Pedir a casa inteira num prompt só faz o modelo inventar janela e trocar um cômodo por outro. Aqui cada trecho do voo fica preso entre duas imagens: a foto real do cômodo vazio e uma versão mobiliada feita em cima da mesma foto. O fim de um trecho é o começo do próximo, e é isso que deixa o voo contínuo.

## O que você precisa

- Claude Code.
- O conector MCP da Higgsfield ligado, com créditos.
- De 4 a 10 fotos do imóvel: a fachada e uma por cômodo.
- Permissão para usar as fotos.

## Instalar

```bash
git clone https://github.com/jcarlosamorim/passeio-de-drone ~/.claude/skills/passeio-de-drone
```

## Usar

Abra o Claude Code na pasta onde estão as fotos e digite:

```text
/passeio-de-drone
```

A skill lê as fotos, limpa marca d'água, mobilia cada cômodo, mostra o custo e pede o seu sim antes de gerar. Primeiro sai um rascunho em 480p. O final em 1080p só sai dos trechos que você aprovar.

## Quanto custa

Medido em 23/11/2026, num apartamento de quatro cômodos (dez trechos, 42 segundos):

| Etapa | Créditos |
|---|---|
| Limpar 9 fotos | 18 |
| 4 imagens mobiliadas | 8 |
| Rascunho em 480p | 126 |
| Final em 1080p | 672 |

Os preços mudam. Vale o número que o conector mostrar no dia.

## O que é real e o que é simulação

Paredes, janelas e piso vêm das fotos. Móveis, luz e o caminho entre os cômodos são simulação. As fotos não mostram a planta, então os corredores são inventados. Se o vídeo for para um anúncio, isso precisa estar escrito.

## Estado

A skill registra o método que funcionou numa sessão real, depois de duas tentativas que falharam. Ainda não foi rodada do zero por outra pessoa. Se algo quebrar, abra uma issue.

## Licença

MIT.
