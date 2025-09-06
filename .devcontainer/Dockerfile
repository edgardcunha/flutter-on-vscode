# Use a imagem oficial do Dart como base
FROM dart:latest

# ---
# 1. Instalação de dependências do sistema
# ---

# Inclui Java Development Kit (JDK) 11, necessário para o Android SDK
RUN apt-get update && apt-get install -y \
    curl git unzip xz-utils zip libglu1-mesa nano wget \
    clang cmake ninja-build pkg-config libgtk-3-dev mesa-utils \
    openjdk-11-jdk

RUN apt-get clean

# ---
# 2. Instalação do Flutter e configuração do PATH
# ---
RUN git clone https://github.com/flutter/flutter.git -b stable --depth 1 /flutter
ENV PATH="/flutter/bin:$PATH"

# ---
# 3. Instalação e configuração do Android SDK
# ---

# Define a variável de ambiente para o Android SDK
ENV ANDROID_HOME=/opt/android-sdk

# Cria o diretório para o SDK e concede permissões
RUN mkdir -p ${ANDROID_HOME}/cmdline-tools

# Baixa e descompacta as ferramentas de linha de comando do SDK
# O "&&" garante que o próximo comando só será executado se o anterior for bem-sucedido
RUN wget https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip -O android-cmdline-tools.zip \
    && unzip android-cmdline-tools.zip -d /tmp/cmdline-tools \
    && mv /tmp/cmdline-tools/cmdline-tools ${ANDROID_HOME}/cmdline-tools/latest \
    && rm android-cmdline-tools.zip

# Adiciona as ferramentas do SDK ao PATH
ENV PATH="$PATH:${ANDROID_HOME}/cmdline-tools/latest/bin"
ENV PATH="$PATH:${ANDROID_HOME}/platform-tools"

# Aceita as licenças do SDK de forma não interativa
RUN yes | sdkmanager --licenses

# Instala as plataformas e ferramentas essenciais do SDK
RUN sdkmanager "platform-tools" "platforms;android-34" "build-tools;34.0.0"

# ---
# 4. Verificação final
# ---
RUN flutter doctor -v