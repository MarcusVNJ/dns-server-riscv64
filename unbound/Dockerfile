FROM riscv64/debian:rc-buggy-20250520

ENV DEBIAN_FRONTEND=noninteractive

# Instale o Unbound e dependências
RUN apt update && \
apt install -y unbound unbound-host dnsutils dns-root-data wget && \
apt clean && \
rm -rf /var/lib/apt/lists/*


RUN mkdir -p /var/lib/unbound /run/unbound && \
chown -R unbound:unbound /var/lib/unbound /run/unbound && \
chmod -R 750 /var/lib/unbound /run/unbound

RUN wget https://www.internic.net/domain/named.root -qO- | tee /usr/share/dns/root.hints

RUN ln -s /usr/share/dns/root.key /var/lib/unbound/root.key

RUN chmod 644 /var/lib/unbound/root.key /usr/share/dns/root.hints

# Copie um arquivo de configuração personalizado do Unbound (opcional)
COPY unbound.conf /etc/unbound/unbound.conf.d/unbound.conf

# Exponha a porta DNS padrão
EXPOSE 53/udp
EXPOSE 53/tcp

# Comando para iniciar o Unbound em primeiro plano e com verbosidade
CMD ["unbound", "-d", "-vvvvv"]
