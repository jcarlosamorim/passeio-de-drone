---
name: passeio-de-drone
description: "Transforma as fotos de um imóvel vazio num passeio de drone de 30 segundos, em que cada cômodo aparece vazio e vira mobiliado, pelo conector MCP da Higgsfield (Nano Banana para as imagens, Seedance 2.5 para o vídeo). Trava cada cômodo na foto real para o modelo não inventar janela nem porta, gera rascunho barato em 480p, a pessoa aprova e só então sai o final em 1080p. Use em /passeio-de-drone ou quando pedirem 'faz um tour de drone desse imóvel', 'flyby do apartamento', 'vídeo de imóvel com essas fotos' ou 'mobilia esse imóvel vazio em vídeo'. NÃO use para gerar um imóvel que não existe, para editar um vídeo já gravado, para só mobiliar uma foto (isso é uma imagem, não o passeio) nem sem fotos do imóvel. Gasta créditos da Higgsfield: nada é gerado sem o sim da pessoa, com o custo na pergunta."
---

# passeio-de-drone

Fotos de anúncio entram, um voo contínuo de drone sai. O que segura o resultado é uma regra: **o prompt descreve, a foto trava.** Pedir a casa inteira num prompt só faz o modelo inventar janela e trocar um cômodo por outro. Aqui cada cômodo fica preso entre duas imagens.

Nasceu de uma sessão real, em 23/11/2026: duas tentativas com um prompt único falharam (marca d'água copiada, janela inventada na cozinha, o quarto virou outro cômodo) e a terceira, em trechos travados, passou.

## O que você precisa

- De 4 a 10 fotos do imóvel: a fachada e uma por cômodo. Vazio ou mobiliado, tanto faz.
- Permissão para usar as fotos. Foto sua ou que o dono liberou.
- Opcional: uma foto da vista ou do entorno, para a abertura.
- O conector MCP da Higgsfield ligado. Se não estiver, pare e diga como ligar.

## Passo a passo

1. **Ler as fotos.** Abra cada uma e anote, por cômodo: quantas janelas e portas, em que parede, o piso, o teto. Essa lista vira a régua da conferência. Escolha a ordem do voo: entorno, fachada, depois os cômodos na ordem em que alguém andaria pela casa.
2. **Limpar.** Marca d'água, logo de imobiliária, cursor de mapa ou data na foto aparecem no vídeo, mesmo com o prompt proibindo. Limpe antes, uma imagem por foto, com o prompt 1. Confira que nada mais mudou.
3. **Mobiliar em cima da foto.** Para cada cômodo, gere a versão mobiliada a partir da própria foto limpa, com o prompt 2. Depois ponha a foto e a versão mobiliada lado a lado e confira a régua do passo 1: mesma janela, mesma porta, mesmo piso. Uma que mudou se refaz sozinha.
4. **Planejar os trechos.** O voo é uma corrente em que o fim de um trecho é o começo do próximo:
   - entorno → fachada (6 s)
   - fachada → cômodo 1 vazio (4 s)
   - cômodo vazio → o mesmo cômodo mobiliado (4 s), com um clarão no meio
   - cômodo mobiliado → próximo cômodo vazio (4 s), saindo por uma porta
   - último cômodo mobiliado → saída para a área externa (4 s)
   Com quatro cômodos são dez trechos e 42 s.
5. **Custo e sim.** Estime com o conector antes de gerar e mostre o total: imagens, rascunho e final. Referência medida em 23/11/2026: 2 créditos por imagem, 3 créditos por segundo de rascunho em 480p, 16 por segundo em 1080p. Vale o número que o conector devolver no dia. Sem o sim, nada é gerado.
6. **Rascunho em 480p.** Seedance 2.5, modo `omni_reference`, `draft: true`, cada trecho com `start_image` e `end_image` e o prompt 3 ou 4. Mande todos de uma vez.
7. **Conferir o rascunho.** Tire quadros de cada trecho e compare com as fotos: janela ou porta que não existe, cômodo que mudou, pessoa, texto ou marca em quadro. Trecho ruim se refaz sozinho. Se um trecho for recusado pelo filtro de direitos autorais, troque a imagem de início por uma foto do próprio imóvel.
8. **Final em 1080p.** Finalize só os trechos aprovados, com `draft_job_id` e a resolução. O final sai igual ao rascunho. O prompt não pode ir vazio.
9. **Montar.** Junte os trechos na ordem, tirando o primeiro quadro de cada um a partir do segundo (é igual ao último do anterior). Para fechar em 30 s, acelere 1,4 vez. Entregue o arquivo de 30 s e o de 42 s.
10. **Dizer o que é real.** Na entrega, diga em uma linha: paredes, janelas e piso vêm das fotos; móveis, luz e o caminho entre os cômodos são simulação. As fotos não mostram a planta, então os corredores são inventados. Em anúncio, isso precisa estar escrito.

## Prompts

**1 · Limpar a foto**

```text
Remove the faint watermark overlay in the center of this photo and restore what is behind it. Change nothing else: same room, same windows, same framing, same light, same colors.
```

**2 · Mobiliar em cima da foto**

```text
Virtual staging of this exact photo, same camera position and lens, pixel-aligned. Keep every wall, window, door, ceiling and floor exactly where it is and exactly the same shape. Do not add any window or door. Only add furniture and change the light: it is dusk, deep blue sky outside, the room lit by warm lamps. Add [furniture for this room]. No people, no animals, no text, no logos.
```

**3 · Trecho de clarão (cômodo vazio → mobiliado)**

```text
Same room, same camera position, a slow gentle drone hover drifting slightly forward. The empty room in daylight holds for a moment, then one quick bright flash of light and the same room is at dusk with [furniture], ending exactly on the last frame. Walls, windows, doors, ceiling and floor never change. No people, no text, no logos. Audio: no music; quiet room tone and one short soft impact on the flash.
```

**4 · Trecho de travessia (um ambiente → o próximo)**

```text
One continuous fast FPV drone shot, wide action camera, photorealistic. From [where it starts] the drone flies out through [door or opening], along a short plain hallway where the light brightens back to daylight, and into [next room], ending exactly on the last frame. No cuts, no dissolves. The room is empty; its only openings are [windows and doors from the photo]. No people, no text, no logos. Audio: no music; a soft drone whoosh and quiet room tone.
```

## Regras

- **Nada de texto dentro da geração.** Endereço, metragem e nome de cômodo entram depois, na edição. O modelo erra a grafia em português.
- **Sem pessoas e sem música**, salvo pedido. Só efeito e ambiente, para sonorizar depois.
- **Uma foto por cômodo, no começo e no fim do take.** É isso que impede janela inventada.
- **Rascunho antes do final, sempre.** O rascunho custa cerca de um quinto.
- **Não mexer no imóvel.** Móvel e luz podem. Janela, porta, parede e piso não.
- **Honestidade na entrega.** Dizer o que é simulação e o que não foi conferido.
