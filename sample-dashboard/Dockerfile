# 公式からpython3.8.1-slimイメージをpull
FROM python:3.8.1-slim

# 作業ディレクトリを設定
WORKDIR /usr/src/sample-dashboard/

# 環境変数を設定
# Pythonがpyc filesとdiscへ書き込むことを防ぐ
ENV PYTHONDONTWRITEBYTECODE 1
# Pythonが標準入出力をバッファリングすることを防ぐ
ENV PYTHONUNBUFFERED 1

# pipの更新
RUN pip install --upgrade pip==20.1 && pip install poetry

COPY ./sample-dashboard/*.toml ./sample-dashboard/poetry.lock ./

RUN poetry install

# Pipenvをインストール
# RUN pip install --upgrade pip \
# && pip install pipenv

# # ホストのpipfileをコンテナの作業ディレクトリにコピー
# COPY ./Pipfile /usr/src/app/Pipfile

# # pipfileからパッケージをインストールしてDjango環境を構築
# RUN pipenv install --skip-lock --system --dev

# ホストのカレントディレクトリを作業ディレクトリにコピー
COPY ./sample-dashboard /usr/src/sample-dashboard
