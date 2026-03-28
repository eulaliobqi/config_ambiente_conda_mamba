cat > ~/ambiente_conda_bioinfo/README.md << 'EOF'
# ambiente_conda_bioinfo

Documentação e ambientes Conda para bioinformática em Debian 14 (amd64).

Projeto desenvolvido para configuração reproduzível de ambientes de análise
nas áreas de genômica, transcriptômica, montagem de genomas, filogenia
e biologia estrutural.

---

## Requisitos de sistema

- Debian 14 amd64 (kernel 6.19.8+)
- Acesso sudo
- Conexão com a internet
- Miniforge3 + Mamba

---

## Estrutura do repositório
```
ambiente_conda_bioinfo/
├── README.md
├── .gitignore
├── envs/
│   ├── ngs.yml            # ambiente completo com versões pinadas
│   ├── ngs-minimal.yml    # apenas pacotes instalados explicitamente
│   ├── trinity.yml        # montagem de transcriptoma de novo
│   ├── phylo.yml          # filogenia e evolução molecular
│   ├── struct.yml         # biologia estrutural
│   └── renv.yml           # análise de expressão diferencial (R)
├── docs/
│   ├── 00_preparacao_sistema.md   # atualização e pacotes base do Debian
│   ├── 01_instalacao_mamba.md     # instalação do Miniforge3 + Mamba
│   ├── 02_configuracao_git.md     # Git e GitHub CLI
│   └── 03_ambiente_ngs.md         # criação e uso do ambiente ngs
└── scripts/
    └── setup.sh           # instalação automatizada de todos os ambientes
```

---

## Início rápido

### 1. Preparar o sistema
```bash
# Atualizar e instalar dependências base
# Ver: docs/00_preparacao_sistema.md
sudo apt update && sudo apt full-upgrade -y
sudo apt install -y build-essential git curl wget \
  zlib1g-dev libbz2-dev liblzma-dev libssl-dev \
  libcurl4-openssl-dev python3 python3-pip \
  default-jre default-jdk htop tmux pigz
```

### 2. Instalar o Miniforge3
```bash
# Ver: docs/01_instalacao_mamba.md
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
bash Miniforge3-Linux-x86_64.sh
source ~/.bashrc
```

### 3. Configurar channels
```bash
conda config --remove channels defaults 2>/dev/null || true
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict
```

### 4. Clonar este repositório
```bash
git clone https://github.com/eulalio/ambiente_conda_bioinfo.git
cd ambiente_conda_bioinfo
```

### 5. Recriar um ambiente
```bash
# Ambiente NGS
mamba env create -f envs/ngs.yml
conda activate ngs

# Ou usar o script automatizado para todos os ambientes
bash scripts/setup.sh
```

---

## Ambientes disponíveis

| Ambiente  | Python | Status | Descrição                              |
|-----------|--------|--------|----------------------------------------|
| `ngs`     | 3.11   | ✓ ativo | NGS, RNASeq, montagem, anotação       |
| `trinity` | 3.8    | planejado | Transcriptoma de novo              |
| `phylo`   | 3.11   | planejado | Filogenia e evolução molecular     |
| `struct`  | 3.10   | planejado | Biologia estrutural               |
| `renv`    | R 4.3  | planejado | Expressão diferencial (R)         |

---

## Ferramentas instaladas

### ngs

| Ferramenta | Versão | Função                         |
|------------|--------|--------------------------------|
| fastqc     | 0.12.1 | Controle de qualidade de reads |

> A tabela é atualizada a cada nova instalação.

---

## Fluxo de atualização

Sempre que uma nova ferramenta for instalada:
```bash
conda activate ngs
mamba install -n ngs -y <ferramenta>

# Atualizar arquivos de ambiente
mamba env export > envs/ngs.yml
mamba env export --from-history > envs/ngs-minimal.yml

# Commitar e publicar
git add .
git commit -m "feat(ngs): adicionar <ferramenta>"
git push
```

---

## Documentação

| Arquivo                        | Conteúdo                              |
|--------------------------------|---------------------------------------|
| `docs/00_preparacao_sistema.md`| Atualização e pacotes base do Debian  |
| `docs/01_instalacao_mamba.md`  | Instalação do Miniforge3 + Mamba      |
| `docs/02_configuracao_git.md`  | Git e GitHub CLI                      |
| `docs/03_ambiente_ngs.md`      | Criação e uso do ambiente ngs         |

---

## Sistema testado
```
OS:      Debian GNU/Linux 14 (amd64)
Kernel:  6.19.8+deb14-amd64
Conda:   26.1.1
Mamba:   2.5.0
Python:  3.11.15 (env ngs)
```

---

## Licença

MIT — livre para usar, modificar e distribuir.
EOF
