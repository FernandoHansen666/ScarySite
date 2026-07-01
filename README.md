# ScarySite

Site oficial e **web flasher** do projeto [Scary-RF](https://github.com/FernandoHansen666/ScaryRF-315-433mhz) — uma ferramenta de pesquisa em RF Sub-GHz baseada em ESP32 + CC1101.

🌐 **Online:** https://scary.com.br

---

## O que é

Site estático (HTML/CSS/JS puro, sem build) publicado via **GitHub Pages**. Tem duas páginas:

| Página | Arquivo | Função |
|---|---|---|
| Landing page | [`index.html`](index.html) | Apresenta o projeto: features, hardware, instalação e links. Fundo em vídeo, tema gótico, seletor de idioma EN/PT. |
| Web Flasher | [`flesh.html`](flesh.html) | Grava o firmware no ESP32 direto pelo navegador via [ESP Web Tools](https://esphome.github.io/esp-web-tools/), com seletor de versão. |

---

## Estrutura

```
.
├── index.html            # landing page
├── flesh.html            # web flasher (ESP Web Tools)
├── styles.css            # estilos da landing page
├── CNAME                 # domínio custom (scary.com.br)
├── MSHM.mp4              # vídeo de fundo
├── Scary_LOGO.svg        # logo + ícones das redes
├── assets/fonts/         # fontes Hemogoblin e alagard
└── firmware/             # binários do firmware, versionados
    ├── v1_NoSD/          # manifest.json + .bin
    ├── v2_NoSD/          # manifest.json + .bin
    ├── v3/               # manifest.json + .bin  (versão atual/padrão)
    ├── v3.1/             # (reservado — futuras atualizações da linha v3)
    └── alpha/            # (reservado)
```

---

## Web Flasher e versionamento

Cada versão do firmware vive em `firmware/<versão>/` com o seu próprio `manifest.json` apontando para os `.bin` daquela pasta. O `<select>` em [`flesh.html`](flesh.html) troca qual `manifest` o ESP Web Tools usa ao gravar.

### Adicionar uma versão nova

1. Gere os binários no Arduino IDE (**Sketch → Export Compiled Binary**) e copie para `firmware/<versão>/`:
   - `*.ino.bootloader.bin` (offset `0x1000`)
   - `*.ino.partitions.bin` (offset `0x8000`)
   - `*.ino.bin` (offset `0x10000`)
2. Crie um `manifest.json` na pasta (copie de outra versão e ajuste `"version"` e os `path`).
3. Em [`flesh.html`](flesh.html), adicione uma `<option>` no `<select>` apontando para o novo `manifest.json` e marque-a como `selected` se for a mais recente (removendo o `selected` da anterior). Atualize também o atributo `manifest` do `<esp-web-install-button>`.

> 💡 O **bootloader** não depende do código, só das configs de build (Flash Mode/Frequency/Size). Se a placa e as configs forem as mesmas, dá para reaproveitar o `bootloader.bin` de uma versão anterior.

> A linha **v3** será atualizada aos poucos como `v3.1`, `v3.2`, ... — agrupe os point-releases num `<optgroup label="v3">`.

---

## Rodar localmente

O flasher **não funciona abrindo o arquivo direto** (`file://`) — o ESP Web Tools precisa baixar o `manifest.json` via HTTP e o Web Serial exige contexto seguro. Suba um servidor local:

```bash
# Python
python -m http.server 8000

# ou Node
npx serve
```

Depois abra `http://localhost:8000/` (landing) ou `http://localhost:8000/flesh.html` (flasher).

---

## Requisitos para gravar

- Navegador **Chrome ou Edge no desktop** (Web Serial não existe no Firefox, Safari nem em celular).
- Site servido por **HTTPS** ou `localhost`.
- ESP32 conectado via USB.

---

## Deploy

Publicado por **GitHub Pages** a partir da branch `main`, com domínio custom `scary.com.br` (arquivo [`CNAME`](CNAME)). Após o push na `main`, o Pages leva ~1 min para atualizar.

---

## Créditos

Projeto Scary-RF por [FernandoHansen666](https://github.com/FernandoHansen666). Instalador via [ESP Web Tools](https://esphome.github.io/esp-web-tools/).
