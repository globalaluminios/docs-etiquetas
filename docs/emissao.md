---
title: Emissão de NFes
summary: Este guia foi desenvolvido para fornecer diretrizes abrangentes e práticas para o processo de impressão de etiquetas e expedição de produtos em nossa empresa.
authors:
    - Gabriel Nascimento
date: 2023-12-23
---
!!! info "Horários da automação"
    A automação de NFe roda duas vezes ao dia nos horários **05:30h da manhã** e às **15:00h**.  
    <sub>Horário de Brasília (GMT -3)</sub>

A preparação e emissão de NFes na Global é feita quase inteiramente pela automação nas contas **Mundial Alumínios** e **Imperium Alumínios** e dividido em duas partes, de acordo com o seguinte fluxograma:
### Preparação de NFes (automação)
```mermaid
graph TD
A(Puxar lista de pedidos ''Em aberto'') --> B[Colocar 90% em cada um]
B --> C{{Erros na validação?}}
C -- Sim --> D(Mover para ''Aguardando verificação'')
C -- Não --> E[Mover para ''Gerado NF - Automação'']
E --> F(Sistema envia NF p/ fila de pendentes)
```
!!! tip "Dica"
	Quando falamos em "Mover para *X*", estamos falando de diferentes situações existentes no sistema do Bling. Essas situações são a maneira que acompanhamos os pedidos no sistema e o que vai acontecendo com eles. São fases importantes e devem ser acompanhadas pelo colaborador responsável pelo faturamento. Entenda mais em [Situações dos pedido de venda – Bling!](https://ajuda.bling.com.br/hc/pt-br/articles/360036457114-Situa%C3%A7%C3%B5es-dos-pedido-de-venda)
	
O processo de colocar 90% nos pedidos é necessário para garantir compliance com as regras de faturamento da empresa e todo pedido dentro das lojas *Shein* e *Shopee* devem ser modificados e faturados de acordo com essa condição.
Após a automação fazer esse processo, ele parte para o faturamento de facto das NFs. Isso também é automático.

### Faturamento
```mermaid
graph TD
A(Puxar lista de NFs ''Pendentes'') --> B[Enviar p/ autorização na Sefaz]
B --> C{{Erros na autorização?}}
C -- Sim --> D(NF continua na fila de pendentes)
C -- Não --> E[NF sobe para Sefaz]
E --> F(Sistema envia NF p/ plataforma)
```
Quando uma NF sobe para a Sefaz, na maioria dos casos, ele já envia os dados dela para a plataforma fazer a validação, de modo que a etiqueta de envio do pedido já estará disponível no **[canal de origem](/glossario/o-que-e-canal-de-origem)**.

Você pode acompanhar o trabalho da automação no seguinte repositório privado do Github: [Workflow runs · gbrnasc/Script-90off (github.com)](https://github.com/gbrnasc/Script-90off/actions)  
Lembrando que é necessário pedir autorização e ter uma conta no Github para poder acompanhar.

**Nessa parte do processo, são necessárias as verificações para policiar o trabalho da automação.** Como já foi explicado nos fluxogramas acima, ela não é perfeita e caso haja quaisquer erros na validação do pedido ou na autorização da NF, a automação jogará o pedido para uma fila de pendências. É importante que o colaborador responsável verifique essa fila, seguindo algumas regras e soluções básicas importantes:
## Verificação de pendências - Pedidos
### Para pedidos em "Aguardando verificação"
=== "NF já foi gerada"
    Para pedidos que a NF já foi gerada e já está integrada na plataforma (símbolo de <img src="/assets/bling-dollar.png" alt="ícone de dólar" style="height: 15px; width: 15px;"></img> no pedido) você pode mover direto para a situação ":fontawesome-solid-circle:{.atendido} Atendido", com a anotação "NF já gerada"
=== "NF ainda não foi gerada"
    Para pedidos em que a NF não foi nem sequer gerada, você pode reintegrar o pedido movendo ele para a situação ":fontawesome-solid-circle:{.aberto} Em aberto" com a anotação "Reintegrando"

    Você consegue saber se a NF for gerada se o pedido não tiver nem o símbolo de dólar <img src="/assets/bling-dollar.png" alt="ícone de dólar" style="height: 15px; width: 15px;"></img> e nem o símbolo de NF <img src="/assets/bling-nf.png" alt="ícone de dólar" style="height: 15px; width: 15px;"></img>.

## Verificação de pendências - NF
Para as NFs que estão na situação "Pendentes", há algumas verificações que podem ser feitas:
???+ warning "Como descobrir o erro da NF?"
    Você deve seguir o seguinte procedimento para verificar qual é o erro da nota:  
        1. Clique na nota fiscal  
        2. Clique em 'Salvar'  
    Esse procedimento é necessário para buscar o erro desta nota.


=== "Faltando bairro"
    Para as NFs que estejam faltando o bairro, você pode solucionar colocando "Centro" no contato, de acordo com o guia abaixo:  
    Você pode clicar no campo que está faltando (nesse caso, o bairro) para poder editar as informações de contato. 
    ![Tutorial de como colocar o bairro](/assets/tutorial-bairro1.webp)
    Após isso, você deve preencher **"Centro"** (independentemente do bairro de fato), clicar em "Utilizar este cadastro" e ir salvar a venda
    ![Tutorial do bairro - pt. 2](/assets/tutorial-bairro2.webp)
=== "Faltando NCM"
    NCM significa _Nomenclatura Comum do Mercosul_ e é um código criado para facilitar e padronizar o comércio de produtos dentro da zona do Mercosul. Nosso NCM por padrão (como só comercializamos **um** tipo de produto) é _**76.15.10.00**_
    As vezes alguns produtos são importados sem NCM para a venda, causando esse erro.
    Para erros que faltam o NCM, você pode seguir o tutorial abaixo:
    <iframe src="https://scribehow.com/embed/Add_a_new_NCM_code_for_Bling_invoices__biYsn7bWRu29pUvQq5T3sQ?skipIntro=true" width="100%" height="640" allowfullscreen frameborder="0"></iframe>

=== "Faltando origem"
    Todos os produtos em NF deve ter a sua **Origem** especificada neles. As vezes, alguns produtos são importados sem essa origem na venda.  
    Para consertar esse erro, você pode seguir o tutorial abaixo:
    <iframe src="https://scribehow.com/embed/Adicionando_a_origem_nas_NFs__7BcW78jPS_WGz5kxQnvNyg?skipIntro=true" width="640" height="640" allowfullscreen frameborder="0"></iframe>
=== "Consultar situação"
    Devido à lentidão de SEFAZ em alguns momentos do dia, algumas notas fiscais podem ficar paradas na situação ```Aguardando protocolo```.  
    Para resolver esses erros, é bem simples:
    <iframe src="https://scribehow.com/embed/Consultando_recibos_de_NFs_pendentes___1eFTY7jQGioSidXmmBriw?skipIntro=true" width="640" height="640" allowfullscreen frameborder="0"></iframe>

## Outros tipos de rejeição e falhas de validação
O Bling pode enviar outros erros de validação e rejeição que não estão listados aqui. Para isso, acesse a base de ajuda do Bling em:  
[Notas Fiscais e Tributações > Erros e Rejeições](https://ajuda.bling.com.br/hc/pt-br/sections/360005474554-Erros-e-Rejei%C3%A7%C3%B5es)

## Conclusão
Assim, concluímos o processo de **Emissão de NFes** na Global Alumínios. Acesse a próxima página abaixo para continuar na jornada de aprendizado.