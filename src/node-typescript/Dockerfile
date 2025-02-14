ARG VERSION=22-bookworm
FROM node:${VERSION}

RUN apt-get update && apt-get install -y \
  sudo \
  zsh \
  curl \
  git \
  wget \
  vim \
  lsb-release \
  gnupg \
  ca-certificates \
  docker.io \
  gh \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN echo "node ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/node

RUN chsh -s /usr/bin/zsh node

RUN corepack enable \
  && chmod +x /usr/local/bin/pnpm \
  && chmod +x /usr/local/bin/yarn

USER node

RUN mkdir -p ~/.zsh_plugins && cd ~/.zsh_plugins && \
  git clone https://github.com/zsh-users/zsh-autosuggestions.git && \
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git

RUN curl -fsSL https://starship.rs/install.sh | sh -s -- -y

RUN echo 'eval "$(starship init zsh)"' >> ~/.zshrc && \
  echo 'source ~/.zsh_plugins/zsh-autosuggestions/zsh-autosuggestions.zsh' >> ~/.zshrc && \
  echo 'source ~/.zsh_plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh' >> ~/.zshrc

USER root
RUN groupadd -f docker && usermod -aG docker node

USER node
WORKDIR /workspace

CMD ["zsh"]