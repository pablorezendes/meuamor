# meuamor 💘

Site-homenagem **Pablo & Jhulia** — Dia dos Namorados 2026.
Produção: https://meuamor.tarotdajhu.com.br

Site estático (HTML/CSS/JS puro), servido por nginx atrás do Traefik.

## Deploy no servidor

```bash
cd /srv/stack/meuamor
git clone https://github.com/pablorezendes/meuamor.git .
docker compose up -d
```

### Se o Traefik da stack usar outros nomes

Descubra os nomes usados pelos outros serviços:

```bash
grep -rh -E "entrypoints|certresolver|traefik" /srv/stack/*/docker-compose.y*ml | sort -u | head -20
docker network ls | grep -iE "traefik|proxy|web"
```

E ajuste no `docker-compose.yml` deste repo, se forem diferentes:

| Item        | Valor padrão aqui | Onde mudar                                  |
|-------------|-------------------|---------------------------------------------|
| rede        | `traefik`         | `networks:` (no serviço e no bloco externo) |
| entrypoint  | `websecure`       | label `...routers.meuamor.entrypoints=`     |
| certresolver| `letsencrypt`     | label `...tls.certresolver=`                |

### DNS

Criar o registro no DNS de `tarotdajhu.com.br`:

```
meuamor  →  A  →  IP do servidor (mesmo IP dos outros serviços da stack)
```

O Traefik emite o certificado HTTPS sozinho na primeira visita.

## Atualizar o site

```bash
cd /srv/stack/meuamor && git pull
# não precisa reiniciar nada — o nginx serve os arquivos direto do volume
```

## Estrutura

- `index.html` — o site inteiro (CSS/JS embutidos)
- `fotos/` — fotos do casal + `arcanos/` (cartas Rider-Waite, domínio público)
- `musica/` — as músicas do Pablo (tocadas pelo player do site)
