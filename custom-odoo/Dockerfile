FROM odoo:18

USER root

# 安裝依賴
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    xfonts-75dpi \
    xfonts-base \
    libfontconfig1 \
    libxrender1 \
    && rm -rf /var/lib/apt/lists/*

# 下載並安裝 wkhtmltopdf (Linux 版本，不是 macOS 版本)
RUN wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.buster_amd64.deb \
    && dpkg -i wkhtmltox_0.12.6-1.buster_amd64.deb \
    && rm wkhtmltox_0.12.6-1.buster_amd64.deb

# 安裝字體以解決 Courier 字體問題
RUN apt-get update && apt-get install -y --no-install-recommends \
    fonts-freefont-ttf \
    fonts-liberation \
    && rm -rf /var/lib/apt/lists/*

# 切換回 odoo 用戶
USER odoo