# Herbario Data

Metadados extraídos de bancos de dados.

## Coleções

Recursos/repositórios analisados:
- Brasiliana Museus
- Wikidata
- API do MAC USP

## Dados

### `csv`

#### `macusp.csv`

Arquivo com metadados sobre objetos da coleção do MAC USP.

### `json`

#### `20250705_processed.json`

Metadados extraídos para cada imagem.

Chave principal é o id da imagem. Seguintes chaves apontam para informacões sobre a obra: 
- "captions": descrição e palavras-chaves extraídas por LLMs
- "categories": tipo de obra: pintura, desenho, gravura, etc
- "color_palette": paleta de 4 cores representativas
- "creator": nome do autor/artista
- "date": data de criação da obra (texto)
- "depicts": descrição de objetos e da obra de acordo com wikipedia
- "embeddings": vetores de embeddings
- "id": id da imagem, mesmo que a chave principal
- "image": url e informação sobre o tamanho (proporção: altura/largura) da imagem da obra
- "museum": nome da coleção de onde foi extraída a imagem
- "objects": objetos detectados na imagem
- "title": nome da obra
- "url": link para mais informação sobre a obra
- "year": ano de criação extraído do campo "date"
- "yearp": ano de criação e valor de confiaça extraídos usando um modelo treinado nas obras com datas exatas, 

```json
{
  "5346010": {
    "captions": {
      "gemma3": {
        "en": {
          "all": [ all keywords ],
          "animals": [ keywords ],
          "environment": [ keywords ],
          "fauna": [ keywords ],
          "flora": [ keywords ],
          "nature": [ keywords ],
          "objects": [ keywords ],
          "people": [ keywords ],
          "plants": [ keywords ],
          "unstructure": [ short description of artwork ]
        },
        "pt": { ... },
      }
    },
    "categories": [ "drawing"],
    "color_palette": [
      [ r0, g0, b0 ],
      [ r1, g1, b1 ],
      [ r2, g2, b2 ],
      [ r3, g3, b3 ],
    ],
    "creator": "Victor Meirelles de Lima",
    "date": "1800-1820",
    "depicts": {
      "en": [ keywords ],
      "pt": [ palavras ]
    },
    "embeddings": {
      "tsne2d": [ x, y ]
    },
    "id": "5346010",
    "image": {
      "url": "https://museu.com/imagem/img_eample.jpg",
      "ratio": 0.67
    },
    "museum": "MASP",
    "objects": [
      { "box": [ x0, y0, x1, y1 ], "label": "human hand", "score": 0.231 },
      { "box": [ x0, y0, x1, y1 ], "label": "horse", "score": 0.31 },
      ...
    ],
    "title": "O Cavalo",
    "url": "https://museu.com/id/1212121",
    "year": 1800,
    "yearp": [
      1818,
      0.887
    ]
  },
  "5362834": {
    ...
  }
}
```

#### `20250705_art-crops.json`

Arquivo com embeddings de `SigLIP2` para todas as porções de imagens com potenciais objetos.

Chaves são formadas pelos ids das imagens e objetos. Por exemplo: `1234_0000` é o primeiro objeto extraído da imagem `1234`. E valores são listas com os 1152 valores de embeddings.

```json
{
  "1234_0000": [ ... embeddings ...],
  "1234_0001": [ ... embeddings ...],
  ...
  "5844_0010": [ ... embeddings ...]
}
```

#### `20250705_clusters.json`

Informação sobre clusters extraídas usando embeddings de `SigLIP2`.

Contém resultado para divisão em 4, 6, 8, 10, 12, 14 e 16 grupos.

Primeira chave do arquivo é o número de grupos extraídos, seguido por informação sobre os grupos e listas de imagens em casa grupo.

Informação sobre as clusters contém palavras chaves usadas para descrever cada grupo, em portugues e ingles, e usando dois modelos e metodologias diferentes de extração, `gemma` e `SigLIP2`.

A lista de imagens é organizada pelos ids das imagens e contém não só o id da cluster principal da imagem, mas a ditância entre o embedding da imagem e os centróides de todos os clusters.

```json
{
  "4": {
    "clusters": {
      "descriptions": {
        "gemma3": {
          "en": [
            [ group 0 words ],
            [ group 1 words ],
            [ group 2 words ],
            [ group 3 words ],
          ],
          "pt": [
            [ descrição grupo 0 ],
            [ descrição grupo 1 ],
            [ descrição grupo 2 ],
            [ descrição grupo 3 ],
          ],
        },
        "siglip2": {
          "en": [ ... ],
          "pt": [ ... ],
        }
      }
    },
    "images": {
      "5346010": {
        "cluster": 2,
        "distances": [ d0, d1, d2, d3 ]
      },
      "5604141": { ... },
      ...
    },
  },

"6": {
    ...
  },

  ...

  "16" : {
    ...
  }
}
```
