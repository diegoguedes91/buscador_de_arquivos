# Curso de Python 3 do Básico ao Avançado 
#### Desafio: Buscador de arquivos

Criar um programa que faça a pesquisa de arquivos. É fornecido o caminho e um termo de pesquisa. 

O programa ira pesquisar no caminho fornecido, entrando nas pastas e subpastas procurando pelo termo fornecido pelo usuário. O programa irá retornar as informações: 

* Diretório do arquivo 
* Nome 
* Extensão 
* Tamanho 

## Solução: 

```python
from buscadearquivos import pesquisar

caminho = input('Digite o caminho onde o arquivo possa estar: ')
termo = input('Digite o termo da pesquisa: ')

pesquisar(caminho, termo)
```

Conforme o codigo abaixo, foi ultilizado a biblioteca __"os"__ para realizar as pesquisas dos arquivos.</br>
Para fornecer o tamanho na medida correta foi criado a função _formata_tamanho_, que compara o tamanho fornecido pelo programa, divide caso necessário e retorna o nome de medida correta.

```python 
import os

def pesquisar(caminho_procura, termo_procura):

    def formata_tamanho(tamanho):
        base = 1024
        kilo = base
        mega = base ** 2
        giga = base ** 3
        tera = base ** 4
        peta = base ** 5

        if tamanho < kilo:
            texto = 'B'
        elif tamanho < mega:
            tamanho /= kilo
            texto = 'K'
        elif tamanho < giga:
            tamanho /= mega
            texto = 'M'
        elif tamanho < tera:
            tamanho /= giga
            texto = 'G'
        elif tamanho < peta:
            tamanho /= tera
            texto = 'T'
        else:
            tamanho /= peta
            texto = 'P'

        tamanho = round(tamanho, 2)
        return f'{tamanho}{texto}'.replace('.',',')


    contador = 0
    for raiz, diretorios, arquivos in os.walk(caminho_procura):
        for arquivo in arquivos:
            if termo_procura in arquivo:
                try:
                    contador += 1
                    caminho_completo = os.path.join(raiz, arquivo)
                    nome_arquivo, ext_arquivo = os.path.splitext(arquivo)
                    tamanho = os.path.getsize(caminho_completo)
                    print()
                    print('Encontrei o arquivo: ', arquivo)
                    print('Caminho:', caminho_completo)
                    print('Nome:', nome_arquivo)
                    print('Extensão: ', ext_arquivo)
                    print('Tamanho: ', formata_tamanho(tamanho))
                except PermissionError as e:
                    print('Sem permissões de acesso!')
                except FileNotFoundError as e:
                    print('Arquivo não encontrado!')
                except Exception as e:
                    print('Erro desconhecido!')

    print()
    print(f'{contador} arquivo(s) encontrado(s).')
  ``` 
  
  ## Terminal: 
  
  Conforme a imagem abaixo, ao final da pesquisa o programa mostra quantos arquivos foram econtrados com o termo inserido. 
    
   ![exemplo no terminal como o programa se comporta](https://github.com/diegoguedes91/buscador_de_arquivos/blob/main/teminal.png)
