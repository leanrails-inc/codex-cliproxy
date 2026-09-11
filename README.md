# Codex via CartPanda CLIProxyAPI

Configuração para usar o Codex (app desktop e CLI) através do CLIProxyAPI da CartPanda.

É um único arquivo. Não precisa de extensão nem de API key.

## Baixar o Codex

| Sistema | Download |
|---------|----------|
| macOS (Apple Silicon) | [Codex.dmg](https://persistent.oaistatic.com/codex-app-prod/Codex.dmg) |
| Windows (recomendado) | [Microsoft Store](https://apps.microsoft.com/detail/9PLM9XGG6VKS) |
| Windows x64 (instalador direto) | [ChatGPT-x64.msix](https://persistent.oaistatic.com/codex-app-prod/ChatGPT-x64.msix) |
| Windows Arm64 (instalador direto) | [ChatGPT-arm64.msix](https://persistent.oaistatic.com/codex-app-prod/ChatGPT-arm64.msix) |

Página oficial: https://learn.chatgpt.com/docs/app

No macOS, abra o `.dmg` e arraste o Codex para a pasta Aplicativos. No Windows, a versão da Microsoft Store atualiza sozinha; o `.msix` é para quem não consegue usar a Store.

Só quer o CLI? Com Node instalado:

```bash
npm install -g @openai/codex
```

## Requisitos

- Codex instalado (app desktop ou CLI).
- Tailscale conectado. O proxy só é acessível pela Tailnet.
- Login no Codex com sua conta ChatGPT, como de costume.

## Onde colocar o arquivo

Todos os arquivos `.toml` deste repo vão para a MESMA pasta `.codex`:

| Sistema | Pasta de destino          | Arquivos                                                |
|---------|---------------------------|---------------------------------------------------------|
| macOS   | `~/.codex/`               | `config.toml`, `opus.config.toml`, `grok.config.toml`   |
| Windows | `C:\Users\<voce>\.codex\` | `config.toml`, `opus.config.toml`, `grok.config.toml`   |

- `config.toml`: obrigatório. Aponta o Codex para o proxy e define GPT-5.6 Sol como padrão.
- `opus.config.toml` e `grok.config.toml`: opcionais. Perfis para Claude Opus 5 e Grok 4.6 (veja a seção mais abaixo).

A pasta `.codex` é criada na primeira vez que você roda o Codex. Se ainda não existir, crie.

### macOS (Terminal)

```bash
git clone https://github.com/leanrails-inc/codex-cliproxy.git ~/Downloads/codex-cliproxy
mkdir -p ~/.codex
cp ~/Downloads/codex-cliproxy/*.toml ~/.codex/
```

### Windows (PowerShell)

```powershell
git clone https://github.com/leanrails-inc/codex-cliproxy.git "$env:USERPROFILE\Downloads\codex-cliproxy"
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex" | Out-Null
Copy-Item "$env:USERPROFILE\Downloads\codex-cliproxy\*.toml" "$env:USERPROFILE\.codex\"
```

Sem git? Baixe o `config.toml` direto pelo GitHub (botão Download) e copie para o destino acima.

## Se você já tem um config.toml

Não sobrescreva. Abra o `~/.codex/config.toml` e:

1. Adicione `model_provider = "cliproxyapi"` no topo, logo abaixo da linha `model = ...`.
2. Cole o bloco inteiro `[model_providers.cliproxyapi]` do `config.toml` deste repo no final do arquivo.

## Depois de copiar

1. Abra o Codex (app ou terminal) e faça login com a conta ChatGPT se ele pedir.
2. No app desktop, abra uma thread NOVA. Threads abertas antes da mudança continuam no provider antigo.
3. Escolha o modelo no seletor normal (GPT-6 Astra, GPT-5.6 Sol, etc). Não existe seletor separado do cliproxy.

## Teste

```bash
codex exec "Responda exatamente: CLIPROXY OK"
```

Saída esperada: `CLIPROXY OK`.

Para confirmar que o app desktop está usando o proxy, abra o arquivo mais recente em `~/.codex/sessions/` e verifique se a primeira linha contém `"model_provider":"cliproxyapi"`.

## Usar Claude Opus 5 e Grok 4.6 no Codex

O proxy também serve `claude-opus-5` e `grok-4.6`, e o Codex consegue usá-los. Este repo traz dois perfis prontos:

| Arquivo             | Modelo          |
|---------------------|-----------------|
| `opus.config.toml`  | `claude-opus-5` |
| `grok.config.toml`  | `grok-4.6`      |

Copie os dois para a mesma pasta `.codex` (ao lado do `config.toml`):

```bash
# macOS
cp ~/Downloads/codex-cliproxy/opus.config.toml ~/Downloads/codex-cliproxy/grok.config.toml ~/.codex/
```

```powershell
# Windows
Copy-Item "$env:USERPROFILE\Downloads\codex-cliproxy\opus.config.toml","$env:USERPROFILE\Downloads\codex-cliproxy\grok.config.toml" "$env:USERPROFILE\.codex\"
```

### No terminal (CLI)

```bash
codex --profile opus        # Claude Opus 5
codex --profile grok        # Grok 4.6
codex -m claude-opus-5      # alternativa sem perfil
```

### No app desktop

O seletor de modelos do app só mostra modelos oficiais da OpenAI. É um filtro do próprio app, não dá para contornar por configuração. Para usar Opus ou Grok no app:

1. Abra `~/.codex/config.toml` e troque a linha `model =` por `model = "claude-opus-5"` (ou `"grok-4.6"`).
2. Abra uma thread NOVA no app. O seletor vai mostrar "Custom", mas as requisições vão para o modelo escolhido.
3. Para voltar aos GPT, troque a linha `model =` de volta e abra outra thread nova.

Se você usa Opus ou Grok com frequência, o terminal com `--profile` é o caminho mais prático.

## Observações

- O Codex pode avisar que não conseguiu carregar o catálogo de plugins do ChatGPT. É inofensivo.
- Para trocar o modelo padrão, edite a linha `model =`. Modelos GPT disponíveis: gpt-6-astra, gpt-5.6-sol, gpt-5.6-terra, gpt-5.6-luna, gpt-5.5, gpt-5.3-codex-spark.
- Modelos Claude, Gemini e Grok do proxy não aparecem no Codex. Para eles use o pi com a extensão `cartpanda-cliproxy`.
