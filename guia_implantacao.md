# Guia de Implantação do Gerador de Histórias Infantis

Este documento contém instruções para implantar permanentemente o aplicativo Streamlit "Gerador de Histórias Infantis" em diversas plataformas.

## Arquivos Incluídos

1. `app.py` - O código principal do aplicativo Streamlit
2. `praticas_historias_infantis.md` - Documento com as melhores práticas para histórias infantis por faixa etária
3. `requirements.txt` - Lista de todas as dependências Python necessárias

## Opções de Implantação Permanente

### 1. Streamlit Community Cloud (Recomendado para iniciantes)

A maneira mais fácil de implantar um aplicativo Streamlit é usando o [Streamlit Community Cloud](https://streamlit.io/cloud), que é gratuito para aplicativos públicos.

1. Crie uma conta em [share.streamlit.io](https://share.streamlit.io)
2. Crie um repositório GitHub com os arquivos do aplicativo (`app.py`, `praticas_historias_infantis.md` e `requirements.txt`)
3. No Streamlit Community Cloud, clique em "New app" e selecione o repositório
4. Configure as variáveis de ambiente para as chaves de API (opcional)
5. Clique em "Deploy"

### 2. Heroku

1. Crie uma conta no [Heroku](https://heroku.com)
2. Instale o [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
3. Crie um arquivo `Procfile` com o conteúdo: `web: streamlit run app.py --server.port=$PORT`
4. Inicialize um repositório Git e faça o commit dos arquivos:
   ```bash
   git init
   git add app.py praticas_historias_infantis.md requirements.txt Procfile
   git commit -m "Initial commit"
   ```
5. Crie e implante o aplicativo:
   ```bash
   heroku create
   git push heroku master
   ```
6. Configure as variáveis de ambiente para as chaves de API:
   ```bash
   heroku config:set OPENAI_API_KEY=sua_chave_aqui
   ```

### 3. Google Cloud Run

1. Crie uma conta no [Google Cloud](https://cloud.google.com)
2. Instale o [Google Cloud SDK](https://cloud.google.com/sdk/docs/install)
3. Crie um arquivo `Dockerfile`:
   ```dockerfile
   FROM python:3.9-slim
   WORKDIR /app
   COPY requirements.txt .
   RUN pip install -r requirements.txt
   COPY . .
   EXPOSE 8080
   CMD streamlit run app.py --server.port=8080 --server.enableCORS=false
   ```
4. Construa e implante a imagem:
   ```bash
   gcloud builds submit --tag gcr.io/seu-projeto/historias-infantis
   gcloud run deploy historias-infantis --image gcr.io/seu-projeto/historias-infantis --platform managed
   ```
5. Configure as variáveis de ambiente no console do Google Cloud

### 4. AWS Elastic Beanstalk

1. Crie uma conta na [AWS](https://aws.amazon.com)
2. Instale o [AWS CLI](https://aws.amazon.com/cli/) e o [EB CLI](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html)
3. Inicialize o aplicativo Elastic Beanstalk:
   ```bash
   eb init -p python-3.8 historias-infantis
   ```
4. Crie um arquivo `.ebextensions/01_streamlit.config`:
   ```yaml
   option_settings:
     aws:elasticbeanstalk:application:environment:
       PYTHONPATH: "/var/app/current"
     aws:elasticbeanstalk:container:python:
       WSGIPath: "app:app"
   ```
5. Crie um arquivo `Procfile`:
   ```
   web: streamlit run app.py --server.port=5000 --server.enableCORS=false
   ```
6. Implante o aplicativo:
   ```bash
   eb create historias-infantis-env
   ```

## Configuração de Variáveis de Ambiente

Para que o aplicativo funcione corretamente com as APIs dos LLMs, você precisará configurar as seguintes variáveis de ambiente na plataforma escolhida:

- `OPENAI_API_KEY` - Para integração com OpenAI
- `ANTHROPIC_API_KEY` - Para integração com Anthropic
- `AZURE_OPENAI_API_KEY` - Para integração com Azure OpenAI
- `HUGGINGFACE_API_KEY` - Para integração com modelos Meta via LiteLLM (opcional)

## Execução Local

Se preferir executar o aplicativo localmente:

1. Instale as dependências:
   ```bash
   pip install -r requirements.txt
   ```
2. Execute o aplicativo:
   ```bash
   streamlit run app.py
   ```

## Personalização

Você pode personalizar o aplicativo editando o arquivo `app.py`:

- Adicionar ou remover provedores LLM
- Modificar os modelos disponíveis
- Alterar o design da interface
- Ajustar os prompts para cada faixa etária

## Suporte

Se encontrar problemas durante a implantação, consulte a documentação oficial da plataforma escolhida ou a [documentação do Streamlit](https://docs.streamlit.io/).

---

Criado com ❤️ por Manus IA
