```python
"""
# Módulo de gerenciamento de restaurantes

Este módulo contém a classe `Restaurante`, que permite criar e gerenciar
restaurantes, avaliar estabelecimentos, alternar seu estado (ativo/inativo),
exibir o cardápio e listar todos os restaurantes cadastrados.

## Classes:
    - Restaurante: Representa um restaurante e suas funcionalidades.

## Dependências:
    - modelos.avaliacao.Avaliacao
    - modelos.cardapio.item_cardapio.ItemCardapio
"""

from modelos.avaliacao import Avaliacao
from modelos.cardapio.item_cardapio import ItemCardapio

class Restaurante:
    """
    ## Classe para representar um restaurante.
    
    ### Atributos de classe:
        - `restaurantes (list)`: Lista de todos os restaurantes criados.
    
    ### Atributos de instância:
        - `_nome (str)`: Nome do restaurante.
        - `_categoria (str)`: Categoria do restaurante.
        - `_ativo (bool)`: Indica se o restaurante está ativo ou inativo.
        - `_avaliacao (list)`: Lista de avaliações do restaurante.
        - `_cardapio (list)`: Lista de itens do cardápio do restaurante.
    """
    restaurantes = []

    def __init__(self, nome: str, categoria: str):
        """
        ### Inicializa um novo restaurante.

        **Parâmetros:**
            - `nome (str)`: Nome do restaurante.
            - `categoria (str)`: Categoria do restaurante.
        """
        self._nome = nome.title()
        self._categoria = categoria.upper()
        self._ativo = False
        self._avaliacao = []
        self._cardapio = []
        Restaurante.restaurantes.append(self)
    
    def __str__(self) -> str:
        """Retorna uma representação em string do restaurante."""
        return f'{self._nome} | {self._categoria}'
    
    @classmethod
    def listar_restaurantes(cls):
        """
        Exibe a lista de todos os restaurantes cadastrados com suas avaliações e status.
        """
        print(f'{'Nome do restaurante'.ljust(25)} | {'Categoria'.ljust(25)} | {'Avaliação'.ljust(25)} |{'Status'}')
        for restaurante in cls.restaurantes:
            print(f'{restaurante._nome.ljust(25)} | {restaurante._categoria.ljust(25)} | {str(restaurante.media_avaliacoes).ljust(25)} |{restaurante.ativo}')

    @property
    def ativo(self) -> str:
        """Retorna o status do restaurante (ativo ou inativo)."""
        return '⌧' if self._ativo else '☐'
    
    def alternar_estado(self):
        """Alterna o estado do restaurante entre ativo e inativo."""
        self._ativo = not self._ativo

    def receber_avaliacao(self, cliente: str, nota: float):
        """
        Adiciona uma avaliação ao restaurante.

        **Parâmetros:**
            - `cliente (str)`: Nome do cliente que avaliou.
            - `nota (float)`: Nota dada ao restaurante (de 1 a 5).
        """
        if 0 < nota <= 5: 
            avaliacao = Avaliacao(cliente, nota)
            self._avaliacao.append(avaliacao)

    @property
    def media_avaliacoes(self):
        """
        Calcula e retorna a média das avaliações do restaurante.

        **Retorna:**
            - `float`: Média das avaliações arredondada para uma casa decimal, ou '-' caso não haja avaliações.
        """
        if not self._avaliacao:
            return '-'
        soma_das_notas = sum(avaliacao._nota for avaliacao in self._avaliacao)
        quantidade_de_notas = len(self._avaliacao)
        media = round(soma_das_notas / quantidade_de_notas, 1)
        return media

    def adicionar_no_cardapio(self, item: ItemCardapio):
        """
        Adiciona um item ao cardápio do restaurante.

        **Parâmetros:**
            - `item (ItemCardapio)`: Objeto do tipo ItemCardapio a ser adicionado ao cardápio.
        """
        if isinstance(item, ItemCardapio):
            self._cardapio.append(item)
    
    @property
    def exibir_cardapio(self):
        """
        Exibe o cardápio do restaurante com os itens cadastrados.
        """
        print(f'Cardápio do restaurante {self._nome}\n')
        for i, item in enumerate(self._cardapio, start=1):
            if hasattr(item, 'descricao'):
                mensagem_prato = f'{i}. Nome: {item._nome} | Preço: R${item._preco} | Descrição: {item.descricao}'
                print(mensagem_prato)
            else:
                mensagem_bebida = f'{i}. Nome: {item._nome} | Preço: R${item._preco} | Tamanho: {item.tamanho}'
                print(mensagem_bebida)
```
