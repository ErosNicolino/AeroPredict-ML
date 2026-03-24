# Mapeamento do Projeto

> Gerado automaticamente em 24/03/2026, 17:39:36
> Raiz: `C:\Users\Valorant Niko\Documents\AeroPredict-ML`

---

## 📂 Estrutura de Arquivos

```
AeroPredict-ML/
📁 data
  📄 airlines.csv  (359 B)
  📄 airports.csv  (23.3 KB)
  📄 flights.csv  (565.0 MB)
📁 images
📁 notebooks
  📄 01_eda.ipynb  (555.7 KB)
📄 exportar-ia.js  (8.1 KB)
📄 LICENSE.txt  (1.1 KB)
📄 README.md  (2.5 KB)
📄 requirements.txt  (183 B)
```

---

## 📦 Metadados

- **requirements.txt** encontrado ✓
- **Notebooks (.ipynb):** 1

---

## 📄 Conteúdo dos Arquivos

### `data\airlines.csv`
> Tamanho: 359 B

```
[Extensão não mapeada: .csv]
```

---

### `data\airports.csv`
> Tamanho: 23.3 KB

```
[Extensão não mapeada: .csv]
```

---

### `data\flights.csv`
> Tamanho: 565.0 MB

```
[Extensão não mapeada: .csv]
```

---

### `notebooks\01_eda.ipynb`
> Tamanho: 555.7 KB

```json
[Arquivo muito grande para incluir: 555.7 KB]
```

---

### `exportar-ia.js`
> Tamanho: 8.1 KB

```javascript
#!/usr/bin/env node

/**
 * mapear_projeto.js
 *
 * Uso: node mapear_projeto.js [caminho-do-projeto]
 *
 * Exemplo:
 *   node mapear_projeto.js .
 *   node mapear_projeto.js /home/user/AeroPredict-ML
 *
 * Gera: projeto_mapeado.md no diretório atual
 */

const fs = require("fs");
const path = require("path");

// ─── Configuração ─────────────────────────────────────────────────────────────

const ROOT = path.resolve(process.argv[2] || ".");
const OUTPUT = path.join(process.cwd(), "projeto_mapeado.md");

// Extensões de código que terão o conteúdo incluído
const CODE_EXTENSIONS = new Set([
  ".py",
  ".ipynb",
  ".js",
  ".ts",
  ".jsx",
  ".tsx",
  ".r",
  ".R",
  ".sql",
  ".sh",
  ".yaml",
  ".yml",
  ".toml",
  ".cfg",
  ".ini",
  ".env",
  ".txt",
  ".md",
  ".rst",
  ".json",
]);

// Extensões completamente ignoradas (binários, mídia, etc.)
const IGNORE_EXTENSIONS = new Set([
  ".png",
  ".jpg",
  ".jpeg",
  ".gif",
  ".svg",
  ".ico",
  ".webp",
  ".bmp",
  ".pdf",
  ".docx",
  ".xlsx",
  ".pptx",
  ".zip",
  ".tar",
  ".gz",
  ".rar",
  ".7z",
  ".pkl",
  ".pickle",
  ".joblib",
  ".h5",
  ".hdf5",
  ".pt",
  ".pth",
  ".onnx",
  ".parquet",
  ".feather",
  ".arrow",
  ".pyc",
  ".pyo",
  ".pyd",
  ".so",
  ".dll",
  ".exe",
  ".mp4",
  ".mp3",
  ".wav",
  ".avi",
  ".ttf",
  ".woff",
  ".woff2",
  ".eot",
]);

// Pastas completamente ignoradas
const IGNORE_DIRS = new Set([
  ".git",
  "__pycache__",
  ".ipynb_checkpoints",
  "node_modules",
  ".venv",
  "venv",
  "env",
  ".env",
  ".tox",
  "dist",
  "build",
  ".mypy_cache",
  ".pytest_cache",
  ".DS_Store",
  "wandb",
  "mlruns",
  ".idea",
  ".vscode",
]);

// Tamanho máximo de arquivo a incluir (bytes) — evita explodir o .md com CSVs gigantes
const MAX_FILE_BYTES = 100 * 1024; // 100 KB

// ─── Helpers ──────────────────────────────────────────────────────────────────

function humanSize(bytes) {
  if (bytes < 1024) return bytes + " B";
  if (bytes < 1024 ** 2) return (bytes / 1024).toFixed(1) + " KB";
  return (bytes / 1024 ** 2).toFixed(1) + " MB";
}

function langFromExt(ext) {
  const map = {
    ".py": "python",
    ".ipynb": "json",
    ".js": "javascript",
    ".ts": "typescript",
    ".jsx": "jsx",
    ".tsx": "tsx",
    ".r": "r",
    ".R": "r",
    ".sql": "sql",
    ".sh": "bash",
    ".yaml": "yaml",
    ".yml": "yaml",
    ".toml": "toml",
    ".json": "json",
    ".md": "markdown",
    ".rst": "rst",
    ".cfg": "ini",
    ".ini": "ini",
    ".txt": "text",
  };
  return map[ext] || "";
}

// ─── Varredura de arquivos ─────────────────────────────────────────────────────

function walk(dir, depth = 0) {
  let entries = [];
  let items;
  try {
    items = fs.readdirSync(dir, { withFileTypes: true });
  } catch {
    return entries;
  }

  // Ordena: pastas primeiro, depois arquivos, ambos em ordem alfabética
  items.sort((a, b) => {
    if (a.isDirectory() && !b.isDirectory()) return -1;
    if (!a.isDirectory() && b.isDirectory()) return 1;
    return a.name.localeCompare(b.name);
  });

  for (const item of items) {
    if (item.name.startsWith(".") && item.name !== ".env") continue;
    const fullPath = path.join(dir, item.name);
    const relPath = path.relative(ROOT, fullPath);

    if (item.isDirectory()) {
      if (IGNORE_DIRS.has(item.name)) continue;
      entries.push({ type: "dir", name: item.name, relPath, depth });
      entries = entries.concat(walk(fullPath, depth + 1));
    } else {
      const ext = path.extname(item.name).toLowerCase();
      if (IGNORE_EXTENSIONS.has(ext)) continue;
      let size = 0;
      try {
        size = fs.statSync(fullPath).size;
      } catch {}
      entries.push({
        type: "file",
        name: item.name,
        relPath,
        fullPath,
        ext,
        size,
        depth,
      });
    }
  }
  return entries;
}

// ─── Leitura segura de arquivo ─────────────────────────────────────────────────

function readFile(entry) {
  if (entry.size > MAX_FILE_BYTES) {
    return `[Arquivo muito grande para incluir: ${humanSize(entry.size)}]`;
  }
  try {
    return fs.readFileSync(entry.fullPath, "utf-8");
  } catch {
    return "[Não foi possível ler o arquivo (encoding inválido ou binário)]";
  }
}

// ─── Geração do Markdown ───────────────────────────────────────────────────────

function buildTree(entries) {
  let lines = ["```"];
  // Recria a árvore visual
  const rootName = path.basename(ROOT);
  lines.push(rootName + "/");

  for (const e of entries) {
    const indent = "  ".repeat(e.depth);
    const prefix = e.type === "dir" ? "📁 " : "📄 ";
    const size = e.type === "file" ? `  (${humanSize(e.size)})` : "";
    lines.push(`${indent}${prefix}${e.name}${size}`);
  }
  lines.push("```");
  return lines.join("\n");
}

function buildFileContents(entries) {
  const fileEntries = entries.filter((e) => e.type === "file");
  if (fileEntries.length === 0) return "_Nenhum arquivo de código encontrado._";

  const sections = [];

  for (const e of fileEntries) {
    const lang = langFromExt(e.ext);
    const content = CODE_EXTENSIONS.has(e.ext)
      ? readFile(e)
      : `[Extensão não mapeada: ${e.ext}]`;
    sections.push(
      `### \`${e.relPath}\`\n` +
        `> Tamanho: ${humanSize(e.size)}\n\n` +
        `\`\`\`${lang}\n${content}\n\`\`\``,
    );
  }

  return sections.join("\n\n---\n\n");
}

// ─── Coleta de metadados extras ────────────────────────────────────────────────

function collectMeta() {
  const lines = [];

  // requirements.txt / pyproject.toml / setup.py
  for (const dep of [
    "requirements.txt",
    "pyproject.toml",
    "setup.py",
    "environment.yml",
    "Pipfile",
  ]) {
    const p = path.join(ROOT, dep);
    if (fs.existsSync(p)) {
      lines.push(`- **${dep}** encontrado ✓`);
    }
  }

  // Conta notebooks
  function countByExt(dir, ext, acc = 0) {
    let items;
    try {
      items = fs.readdirSync(dir, { withFileTypes: true });
    } catch {
      return acc;
    }
    for (const i of items) {
      if (IGNORE_DIRS.has(i.name)) continue;
      const fp = path.join(dir, i.name);
      if (i.isDirectory()) acc = countByExt(fp, ext, acc);
      else if (i.name.endsWith(ext)) acc++;
    }
    return acc;
  }

  const nbCount = countByExt(ROOT, ".ipynb");
  const pyCount = countByExt(ROOT, ".py");
  if (nbCount) lines.push(`- **Notebooks (.ipynb):** ${nbCount}`);
  if (pyCount) lines.push(`- **Scripts Python (.py):** ${pyCount}`);

  return lines.length
    ? lines.join("\n")
    : "_Nenhum metadado extra encontrado._";
}

// ─── Main ──────────────────────────────────────────────────────────────────────

console.log(`\n🔍 Mapeando projeto em: ${ROOT}\n`);

const entries = walk(ROOT);
const now = new Date().toLocaleString("pt-BR");

const md = `# Mapeamento do Projeto

> Gerado automaticamente em ${now}
> Raiz: \`${ROOT}\`

---

## 📂 Estrutura de Arquivos

${buildTree(entries)}

---

## 📦 Metadados

${collectMeta()}

---

## 📄 Conteúdo dos Arquivos

${buildFileContents(entries)}

---

_Fim do mapeamento._
`;

fs.writeFileSync(OUTPUT, md, "utf-8");

const fileCount = entries.filter((e) => e.type === "file").length;
const dirCount = entries.filter((e) => e.type === "dir").length;

console.log(`✅ Mapeamento concluído!`);
console.log(`   📁 Pastas: ${dirCount}`);
console.log(`   📄 Arquivos: ${fileCount}`);
console.log(`   💾 Gerado: ${OUTPUT}\n`);
console.log(
  `   Agora é só enviar o arquivo "projeto_mapeado.md" para o Claude!\n`,
);

```

---

### `LICENSE.txt`
> Tamanho: 1.1 KB

```text
MIT License

Copyright (c) 2026 Eros Nicolino Da Rocha

Permission is hereby granted, free of charge, to any person obtaining a
copy of this software and associated documentation files (the
“Software”), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS
OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

```

---

### `README.md`
> Tamanho: 2.5 KB

```markdown
# ✈️ AeroPredict --- Flight Delay Prediction Pipeline

[![Status](https://img.shields.io/badge/status-complete-brightgreen)](https://github.com/ErosNicolino/AeroPredict-ML)
[![Python](https://img.shields.io/badge/python-%3E%3D3.10-blue)](https://www.python.org/)
[![Scikit-learn](https://img.shields.io/badge/scikit--learn-%3E%3D1.3.0-orange)](https://scikit-learn.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

---

## 📌 Overview

AeroPredict é um projeto de Machine Learning Engineering focado na
previsão de atrasos de voos utilizando dados estruturados da aviação.

O projeto aplica boas práticas de engenharia:

- Pipeline de pré-processamento sem data leakage\
- Validação cruzada estratificada\
- Comparação de modelos supervisionados\
- Interpretação de variáveis\
- Discussão crítica de limitações

---

## 📂 Dataset

Fonte: Flight Delays and Cancellations (flights.csv)

Target criada:

IS_DELAYED = 1 se ARRIVAL_DELAY \> 15 minutos

---

## 🧹 Data Preprocessing

1.  Remoção de voos cancelados/desviados\
2.  Tratamento de valores ausentes\
3.  OneHotEncoding para categóricas\
4.  Padronização para variáveis numéricas\
5.  Pipeline com ColumnTransformer + Pipeline

---

## 🤖 Modelos

### Logistic Regression

Modelo linear baseline com class_weight='balanced'

### Random Forest

Modelo ensemble não-linear com análise de importância de features

---

## 🔁 Validação

- Train/Test split estratificado\
- Stratified K-Fold (k=5)\
- Métricas:
  - Accuracy
  - Recall
  - ROC-AUC
  - Confusion Matrix
  - ROC Curve

---

## 📊 Performance

Modelo Accuracy Recall ROC-AUC

---

Logistic Regression \~0.59 \~0.62 \~0.63
Random Forest \~0.65 \~0.52 \~0.64

---

## 📊 Visualizações

### ROC Curve

![ROC](/images/viz_cell_4_2.png)

### Feature Importance

![Importance](/images/viz_cell_4_5.png)

### Elbow Method

![Elbow](/images/viz_cell_5_2.png)

### Delay Map

![Map](/images/newplot.png)

---

## 📉 Limitações

- Ausência de dados climáticos externos\
- Dataset limitado a um ano\
- Natureza parcialmente estocástica dos atrasos

---

## 🛠 Tech Stack

- Python\
- Pandas / NumPy\
- Scikit-learn\
- Matplotlib / Seaborn\
- Jupyter

---

## 📁 Estrutura

AeroPredict-ML/
│
├── imagens/
├── notebooks/
├── data/
├── README.md
├── LICENSE
└── requirements.txt

---

## 📎 Como Executar

pip install -r requirements.txt

Abrir notebook e executar.

---

## 👤 Autor

Eros Nicolino da Rocha

---

## 📌 Licença

MIT License

```

---

### `requirements.txt`
> Tamanho: 183 B

```text
# Data Manipulation
pandas>=2.0.0
numpy>=1.24.0

# Visualization
matplotlib>=3.7.0
seaborn>=0.12.0
plotly>=5.15.0

# Machine Learning
scikit-learn>=1.3.0

# Environment
jupyter>=1.0.0
```

---

_Fim do mapeamento._
