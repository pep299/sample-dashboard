# 公式からpython3.8.2-slimイメージをpull
FROM python:3.8.2-slim

# 作業ディレクトリを設定
WORKDIR /usr/src/sample-dashboard/

# 環境変数を設定
# Pythonがpyc filesとdiscへ書き込むことを防ぐ
ENV PYTHONDONTWRITEBYTECODE 1
# Pythonが標準入出力をバッファリングすることを防ぐ
ENV PYTHONUNBUFFERED 1

# Pipenvをインストール
# RUN pip install --upgrade pip \
# && pip install pipenv

# # ホストのpipfileをコンテナの作業ディレクトリにコピー
# COPY ./Pipfile /usr/src/app/Pipfile

# # pipfileからパッケージをインストールしてDjango環境を構築
# RUN pipenv install --skip-lock --system --dev

# ホストのカレントディレクトリ（現在はappディレクトリ）を作業ディレクトリにコピー
COPY ./sample-dashboard /usr/src/sample-dashboard