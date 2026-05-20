# quality_control
Controle de Qualidade para Farmácias de Manipulação
# Guia de Implantação — CQ Fórmula Animal
**Sistema de Controle de Qualidade para Farmácia de Manipulação**

---

## Visão geral

O sistema é um único arquivo HTML que roda em qualquer navegador, sem instalação. Os dados ficam salvos no próprio navegador (localStorage). Para acessar de qualquer dispositivo, você publica o HTML gratuitamente no Vercel.

---

## Etapa 1 — Publicar o sistema no Vercel

### 1.1 Criar conta no Vercel
1. Acesse **vercel.com** e clique em **Sign Up**
2. Escolha **Continue with Google** (mais rápido) ou crie com e-mail
3. Na tela de onboarding, clique em **Skip** se pedir para conectar um repositório

### 1.2 Publicar o arquivo index.html
1. No painel do Vercel, clique em **Add New → Project**
2. Clique em **Browse** ou arraste o arquivo `index.html` diretamente
3. Deixe todas as configurações padrão e clique em **Deploy**
4. Aguarde cerca de 30 segundos
5. O Vercel gera um endereço como `cq-farmacia-xyz.vercel.app` — esse é o endereço do sistema

### 1.3 Personalizar o endereço (opcional)
- No painel do projeto, vá em **Settings → Domains**
- Você pode adicionar um domínio próprio ou mudar o nome para algo como `cq-farmaciaxyz.vercel.app`

> **Para a segunda farmácia:** repita este processo com o mesmo `index.html`. Cada farmácia terá seu próprio endereço Vercel e seus dados são completamente independentes (salvos no navegador de cada farmácia).

---

## Etapa 2 — Converter o histórico de planilhas Excel

### 2.1 Pré-requisitos
- Python 3 instalado no computador
  - Windows: baixe em **python.org/downloads** (marque "Add to PATH" na instalação)
  - Mac: já vem instalado, ou use `brew install python3`
  - Linux: `sudo apt install python3 python3-pip`

### 2.2 Preparar os arquivos
1. Crie uma pasta no computador chamada `converter_cq`
2. Copie para dentro dela:
   - O arquivo `conversor_cq.py` (incluído neste pacote)
   - **Todas as planilhas** `.xlsx` e `.xls` do histórico de CQ
3. Os arquivos precisam ter o **ano no nome** (ex: `CQ_2022.xlsx`, `CQ_Farmacia_2023.xls`)

### 2.3 Executar a conversão
**Windows:**
```
Abra o Prompt de Comando (cmd)
cd C:\caminho\para\converter_cq
python conversor_cq.py
```

**Mac/Linux:**
```
Abra o Terminal
cd /caminho/para/converter_cq
python3 conversor_cq.py
```

O script vai:
- Instalar as dependências automaticamente na primeira execução
- Procurar todas as planilhas na pasta
- Converter as abas "Registro de Entradas" de cada ano
- Salvar o resultado em `historico_cq.json`

**Resultado esperado:**
```
📊 Encontrados 4 arquivo(s):
   - CQ_Farmacia_2022.xlsx
   - CQ_Farmacia_2023.xlsx
   - CQ_Farmacia_2024.xlsx
   - CQ_Farmacia_2025.xls

✅ CQ_Farmacia_2022.xlsx: 87 entradas convertidas (ano 2022)
✅ CQ_Farmacia_2023.xlsx: 142 entradas convertidas (ano 2023)
✅ CQ_Farmacia_2024.xlsx: 198 entradas convertidas (ano 2024)
✅ CQ_Farmacia_2025.xls: 156 entradas convertidas (ano 2025)

🎉 Concluído! 583 entradas salvas em: historico_cq.json
```

### 2.4 Importar no sistema
1. Abra o sistema no navegador
2. Vá em **⚙️ Gerenciar → Importar/Exportar**
3. Clique em **📂 Selecionar historico_cq.json**
4. Escolha o arquivo gerado na etapa anterior
5. Aguarde a mensagem de confirmação

> **Atenção:** faça isso uma vez em cada computador que vai usar o sistema. Os dados ficam armazenados localmente em cada navegador.

---

## Etapa 3 — Banco de substâncias

O sistema já vem com **1505 substâncias** pré-carregadas da planilha de Especificações.

### Se a segunda farmácia tiver substâncias diferentes:
1. Acesse sua farmácia principal no sistema
2. Vá em **⚙️ Gerenciar → Importar/Exportar**
3. Clique em **💾 Exportar Substâncias** — baixa um `substancias_cq.json`
4. Na segunda farmácia: **Importar/Exportar → 📂 Selecionar JSON de substâncias**

### Para adicionar substâncias novas:
- **Manualmente:** ⚙️ Gerenciar → Substâncias → formulário "Adicionar"
- **Com IA (foto/PDF):** botão **🤖 Preencher com IA** no formulário de adição
- **Com IA (texto):** cole uma monografia ou descrição do fornecedor
- **Com IA (CAS):** informe o número CAS da substância

---

## Etapa 4 — Configurar a IA (opcional)

A IA permite analisar laudos por foto ou PDF e preencher formulários a partir de texto.

### 4.1 Obter chave de API
1. Acesse **console.anthropic.com** e crie uma conta
2. Vá em **Billing** e adicione um método de pagamento
3. Compre créditos: **US$ 5** é suficiente para mais de 1 ano de uso típico
4. Vá em **API Keys → Create Key** e copie a chave (começa com `sk-ant-`)

> ⚠️ Não ative a chave de avaliação gratuita (US$ 5 free) se não for usar imediatamente — ela expira em 14 dias.

### 4.2 Configurar no sistema
1. No sistema, vá em **📝 Nova Entrada**
2. Clique em **🤖 Analisar laudo com IA**
3. Cole a chave de API e clique em **Salvar Chave**
4. A chave fica salva no navegador — não precisa repetir

### 4.3 Usar em cada computador
A chave precisa ser configurada uma vez em cada computador/navegador que for usar a funcionalidade de IA.

---

## Etapa 5 — Rotina de backup

Os dados ficam no localStorage do navegador. Se o computador for formatado ou o cache limpo, os dados são perdidos. A rotina de backup evita isso.

### Backup manual (recomendado: semanal)
1. No sistema: **⚙️ Gerenciar → Importar/Exportar**
2. Clique em **💾 Exportar Laudos** → baixa `historico_cq.json`
3. Clique em **💾 Exportar Substâncias** → baixa `substancias_cq.json`
4. Salve os dois arquivos em:
   - Pasta local com data no nome: `backup_cq_2026-05-20/`
   - E-mail para você mesmo
   - Google Drive, OneDrive ou Dropbox da farmácia

### Estrutura de pasta de backup sugerida
```
CQ_Backups/
├── 2026-05/
│   ├── 2026-05-05_historico_cq.json
│   ├── 2026-05-05_substancias_cq.json
│   ├── 2026-05-12_historico_cq.json
│   ├── 2026-05-12_substancias_cq.json
│   ├── 2026-05-19_historico_cq.json
│   └── 2026-05-19_substancias_cq.json
└── 2026-06/
    └── ...
```

### Restaurar um backup
1. No sistema: **⚙️ Gerenciar → Importar/Exportar**
2. Clique em **📂 Selecionar historico_cq.json** e escolha o backup mais recente
3. Clique em **📂 Selecionar JSON de substâncias** e escolha o backup

> O sistema detecta entradas duplicadas e não importa de novo — pode importar sem medo.

### Sincronizar entre computadores da farmácia
Quando quiser que outro computador tenha os mesmos dados:
1. Exporte `historico_cq.json` do computador principal
2. Importe no outro computador
3. Repita sempre que houver novas entradas importantes

---

## Resumo rápido para a segunda farmácia

| Passo | O que fazer | Tempo estimado |
|-------|-------------|----------------|
| 1 | Criar conta no Vercel e publicar o `index.html` | 10 minutos |
| 2 | Copiar planilhas + `conversor_cq.py` para uma pasta, rodar o script | 5 minutos |
| 3 | Abrir o sistema e importar o `historico_cq.json` gerado | 2 minutos |
| 4 | Importar substâncias (exportar da farmácia principal) | 2 minutos |
| 5 | Configurar chave de API se quiser usar IA | 10 minutos |
| **Total** | | **~30 minutos** |

---

## Solução de problemas comuns

**"O conversor deu erro de aba não encontrada"**
→ A aba de registros pode ter um nome diferente. Abra o Excel, veja o nome exato da aba de entradas e edite a linha `if 'entrada' in s.lower() or 'registro' in s.lower()` no script.

**"Os dados sumirram do sistema"**
→ Alguém limpou o cache do navegador. Restaure o backup mais recente.

**"A IA retornou erro de API"**
→ Verifique se a chave de API está correta e se há créditos disponíveis em console.anthropic.com

**"Preciso usar em celular"**
→ Abra o endereço Vercel no celular. Funciona normalmente. Para salvar na tela inicial: no Safari toque em Compartilhar → Adicionar à Tela de Início; no Chrome toque nos 3 pontos → Adicionar à tela inicial.

---

*Guia gerado em maio/2026 · Sistema CQ Fórmula Animal*
