# 🕷️ WebReconSpider

**WebReconSpider** é uma ferramenta de reconhecimento web (recon) baseada em Python e **Scrapy**. Este script atua como um *crawler* que navega por um site alvo para extrair informações valiosas, mapear ativos e identificar vetores de interesse, salvando tudo em um arquivo JSON estruturado.

Ele inclui um **Middleware Personalizado** para lidar corretamente com domínios que utilizam portas não padrão (ex: `http://exemplo.com:8080`).

## Funcionalidades

O script varre o site inicial e segue links internos recursivamente, coletando os seguintes dados:

  * ** E-mails:** Extrai endereços de e-mail encontrados no texto.
  * ** Links:** Mapeia links internos e externos.
  * ** Arquivos Externos:** Identifica links para documentos (PDF, DOCX, XLSX) e folhas de estilo (CSS).
  * ** Arquivos JS:** Lista todos os scripts JavaScript (`.js`) importados.
  * ** Campos de Formulário:** Coleta os nomes (`name`) de inputs, textareas e selects (útil para mapear superfícies de ataque).
  * ** Mídias:** Extrai URLs de Imagens, Vídeos e Áudio.
  * ** Comentários HTML:** Captura comentários ocultos no código fonte (que muitas vezes contêm informações sensíveis ou notas de desenvolvedores).

##  Pré-requisitos

Você precisa ter o **Python 3.6+** instalado. As dependências do projeto são mínimas.

### Instalação

1.  Clone este repositório ou salve o script.
2.  Instale o framework Scrapy:

<!-- end list -->

```bash
pip install scrapy
```

##  Como Usar

Execute o script via linha de comando passando a URL alvo como argumento.

**Sintaxe:**

```bash
python nome_do_script.py <URL_ALVO>
```

**Exemplo:**

```bash
python ReconSpider.py https://www.exemplo.com.br
```

> **Nota:** O crawler está configurado para se restringir ao domínio da URL inicial. Ele não seguirá links para sites externos (apenas registrará que o link existe).

##  Saída (Output)

Após a conclusão da varredura, o script gerará um arquivo chamado `results.json` no mesmo diretório.

Exemplo da estrutura do JSON gerado:

```json
{
    "emails": ["contato@exemplo.com", "suporte@exemplo.com"],
    "links": ["https://www.exemplo.com/sobre", "https://google.com"],
    "external_files": ["https://www.exemplo.com/estilo.css", "https://www.exemplo.com/manual.pdf"],
    "js_files": ["https://www.exemplo.com/app.js", "https://code.jquery.com/jquery.min.js"],
    "form_fields": ["usuario", "senha", "email_newsletter"],
    "images": ["https://www.exemplo.com/logo.png"],
    "videos": [],
    "audio": [],
    "comments": [""]
}
```

##  Detalhes Técnicos

  * **CustomOffsiteMiddleware:** O script substitui o middleware padrão do Scrapy para permitir o rastreamento em URLs que especificam portas (ex: `localhost:8000`), algo que a configuração padrão muitas vezes bloqueia.
  * **Crawling Recursivo:** Ele segue apenas links que pertencem ao mesmo domínio (`netloc`) da URL inicial para evitar sair do escopo.
