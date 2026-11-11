# S5-nano no ENEM 2026: o modelo brasileiro numa prova que ele nunca viu

[![Abrir no Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YasRordrigues/s5nano-enem-2026/blob/main/s5nano_enem2026.ipynb)

A Maritaca AI abriu os pesos do S5-nano (cerca de 4B de parâmetros, Apache 2.0) em 07/11/2026, e a prova do 1º dia do ENEM 2026 foi aplicada no dia seguinte, 08/11. Resultados em provas antigas do ENEM podem estar inflados, porque as questões já circulam na internet e podem ter entrado no treino. Uma prova aplicada depois do lançamento não tem esse problema, então quis ver como o modelo se sai nela.

*Testing Maritaca's new open Brazilian model on the ENEM 2026 exam held the day after its release, so it can't have seen the questions.*

## Como funciona

- O modelo recebe as **páginas escaneadas do caderno azul como imagem**, do jeito que a prova foi publicada, sem transcrição.
- As 90 questões do 1º dia: 45 de Linguagens (nas questões 01 a 05, a opção de inglês) e 45 de Ciências Humanas.
- Um OCR (Tesseract) só localiza em que página está cada "QUESTÃO NN"; quem lê o enunciado e responde é o modelo.
- Prompt único em português pedindo um raciocínio curto e a linha final `Resposta: X`.
- Parâmetros de amostragem recomendados no model card (Instruct: temperature 0.7, top_p 0.8, top_k 20).

## Resultados

![Resultado](card_s5nano_enem2026.png)

Modo Instruct, 1 rodada, GPU L4 no Colab (bf16), cerca de 19 s por questão.

| | Acertos |
|---|---|
| **Total** | **61/90 (67,8%)** |
| Linguagens | 28/45 |
| Ciências Humanas | 33/45 |

- Em 5 questões (07, 21, 70, 72 e 74) o modelo estourou o limite de 1024 tokens antes de escrever a resposta; elas contam como erro.
- O OCR achou o cabeçalho de 84 das 90 questões; nas outras 6 o modelo recebeu as páginas vizinhas e teve que achar a questão sozinho (acertou 4 delas).
- Para comparar: o model card da Maritaca reporta 78,57% no modo Instruct no ENEM-VL, com outras provas e outro formato.

## Limitações

- O gabarito usado é o **extraoficial** da Plataforma Assaad; o oficial do INEP sai até o fim de novembro e os números serão conferidos com ele. Na questão 48 o gabarito extraoficial aceita C ou D.
- Ler a página inteira escaneada é mais difícil do que receber a questão em texto, então o teste mede leitura da prova e raciocínio juntos. Os números da Maritaca no model card usam outro formato e outras provas, então não são diretamente comparáveis.
- Uma rodada só, com amostragem: uma ou duas questões de diferença é ruído.

## Rodar

Abra o notebook no Colab com GPU (T4 ou L4) e execute as células. O notebook baixa o caderno azul publicado pelo Diário do Nordeste, a partir de uma cópia idêntica hospedada pelo FII Brasil (o link do Diário do Nordeste recusa downloads vindos do Colab).

## Créditos

- [maritaca-ai/S5-nano](https://huggingface.co/maritaca-ai/S5-nano)
- Caderno azul: [Diário do Nordeste](https://diariodonordeste.verdesmares.com.br/ceara/enem-2026-baixe-caderno-completo-e-veja-todas-as-questoes-do-1-dia-1.3804325). , cópia em [FII Brasil](https://fiibrasil.com/noticia/gabarito-enem-2026-extraoficial-primeiro-dia-cores-provas/). Prova: INEP.
- Gabarito extraoficial: [Plataforma Assaad](https://plataformaassaad.com.br/gabarito-enem-2026/)
